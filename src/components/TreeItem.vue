<template>
	<li class="tree-item">
		<div class="tree-item-content">
			<img @click="toggleCollapse" v-if="modelValue[this.children]" class="chevron-icon" :class="{'rotate': isCollapsed}" src="../assets/icons/chevron.svg" alt="">
			<Checkbox :options="checkboxOptions" class="tree-item-checkbox" :modelValue="checkboxState" @update:modelValue="val => checkboxStateChanged(val)" />
			<span class="tree-item-text" @click="toggleCollapse">{{ modelValue[this.text] }}</span>
		</div>
		<ul class="tree-list" :class="{'collapse': isCollapsed}" v-if="modelValue[this.children]" ref="treeList">
			<TreeItem v-for="item in modelValue[this.children]" :key="item[this.text]" :checkboxOptions="checkboxOptions" :fields="this.fields" :modelValue="item" @update:modelValue="val => treeItemDataChanged(val)" ref="treeChild" />
		</ul>
	</li>
</template>
<script>
	import Checkbox from './Checkbox.vue'
	export default {
		name: 'TreeItem',
		components: {
			Checkbox
		},
		props: {
			modelValue: {},
			fields: { type: Object },
			checkboxOptions: {}
		},
		data() {
			return {
				halfChecked: false,
				isCollapsed: false,
			}
		},
		computed: {
			children() {
				return this.fields.children
			},
			text() {
				return this.fields.text
			},
			state() {
				return this.fields.state
			},
			checkboxState() {
				if (this.modelValue[this.state].isChecked) {
					return 'checked'
				}
				else if (this.modelValue[this.state].isChecked === false) {
					if (this.halfChecked) {
						return 'halfChecked'
					}
					else {
						return 'unchecked'
					}
				}
			},
		},
		created() {
			this.halfChecked = false
			if (this.modelValue[this.children]?.length > 0) {

				const halfCheckedOrigin = this.modelValue[this.children][0][this.state].isChecked
				for (let i = 0; i < this.modelValue[this.children].length; i++) {
					if (this.modelValue[this.children][i][this.state].isChecked !== halfCheckedOrigin) {
						this.halfChecked = true
					}
				}
			}
		},
		methods: {
			countDifferentStates(item, state) {
				let count = 0
				if (item[this.state].isChecked !== state) {
					count++
				}

				if (item[this.children]) {
					for (let i = 0; i < item[this.children].length; i++) {
						count += this.countDifferentStates(item[this.children][i], state)
					}
				}

				return count
			},
			treeItemDataChanged(val) {
				this.halfChecked = false
				if (this.modelValue[this.children] && this.modelValue[this.children].length > 0) {
					let dataState = true
					const halfCheckedOrigin = this.modelValue[this.children][0][this.state].isChecked
					let valDifferenceCount = 0

					for (let i = 0; i < this.modelValue[this.children].length; i++) {
						if (this.modelValue[this.children][i][this.state].isChecked === false) {
							dataState = false
						}

						valDifferenceCount += this.countDifferentStates(this.modelValue[this.children][i], halfCheckedOrigin)

						if (this.modelValue[this.children][i][this.state].isChecked !== halfCheckedOrigin || valDifferenceCount > 0) {
							this.halfChecked = true
						}
					}
					if (val[this.state].isChecked === false) {
						dataState = false
					}
					let clonedData = this.modelValue
					clonedData[this.state].isChecked = dataState
					this.emit(clonedData)
				}
				this.emit(val)

			},

			changeHalfState(state = false) {
				this.halfChecked = state
			},

			disableAllChildrenHalfStates() {
				this.changeHalfState(false)

				if (this.$refs.treeChild) {
					for (var i = 0; i < this.$refs.treeChild.length; i++) {
						this.$refs.treeChild[i].disableAllChildrenHalfStates()
					}
				}
			},

			checkboxStateChanged(val) {

				this.disableAllChildrenHalfStates()
				this.halfChecked = false
				let clonedData = this.modelValue

				const rec = (items, state) => {
					items.forEach(child => {
						child[this.state].isChecked = state
						if (child[this.children]) {
							child[this.children] = rec(child[this.children], state)
						}
					})
					return items
				}

				if (clonedData[this.children]) {
					clonedData[this.children] = rec(clonedData[this.children], val)
				}

				clonedData[this.state].isChecked = val

				this.emit(clonedData)
			},
			emit(data) {
				this.$emit('update:modelValue', data)
			},
			toggleCollapse() {
				// if (this.modelValue[this.children])
				// 	this.$refs.treeList.classList.toggle('collapse')
				this.isCollapsed = !this.isCollapsed
			},
		}
	}
</script>

<style>
	.tree-item {
		white-space: nowrap;
		display: flex;
		flex-direction: column;
		position: relative;
		box-sizing: border-box;
	}

	.tree-item-content {
		display: flex;
		align-items: center;
		cursor: pointer;
		width: 100%;
		box-sizing: border-box;
		user-select: none;
		position: relative;
		padding-right: 20px;
	}
	.tree-item-text {
		flex: 1;
		height: 100%;
		display: block;
		overflow: auto;
	}

	.tree-item-content:hover {
		background-color: #f6f8fb;
	}

	.tree-list {
		display: none;
		padding: 0px 22px;
		animation: fadeInRight 0.4s ease-in-out;
	}
	.collapse {
		display: block;
	}

	.chevron-icon {
		height: 25px;
		transition: transform 0.4s;
		position: absolute;
		right: 0px;
	}

	.chevron-icon.rotate {
		transform: rotate(-90deg);
	}

	@keyframes fadeInRight {
		from {
			opacity: 0;
			transform: translateX(20px);
		}
		to {
			opacity: 1;
		}
	}
</style>