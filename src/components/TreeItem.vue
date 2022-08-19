<template>
	<li class="tree-item">
		<Checkbox :modelValue="checkboxState" @update:modelValue="val => checkboxStateChanged(val)" />
		<span @click="toggleCollapse">{{ modelValue.name }}</span>
		<ul class="tree-list" v-if="modelValue.children" ref="treeList">
			<TreeItem v-for="item in modelValue.children" :key="item.name" :modelValue="item" @update:modelValue="val => treeItemDataChanged(val)" />
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
			options: { type: Object }
		},
		data() {
			return {
				state: { isChecked: false, isHalfChecked: false },
				// checkboxState: 'unchecked',
			}
		},
		computed: {
			checkboxState() {
				if (this.modelValue.state.isChecked) {
					return 'checked'
				}
				else if (this.modelValue.state.isChecked === false) {
					if (this.modelValue.state.isHalfChecked) {
						return 'halfChecked'
					}
					else {
						return 'unchecked'
					}
				}
			},
		},
		methods: {
			treeItemDataChanged(val) {
				// debugger
				if (this.modelValue.children && this.modelValue.children.length > 0) {
					let dataState = true
					for (let i = 0; i < this.modelValue.children.length; i++) {
						if (this.modelValue.children[i].state.isChecked === false) {
							dataState = false
						}
					}
					if (val.state.isChecked === false) {
						dataState = false
					}
					let clonedData = this.modelValue
					clonedData.state.isChecked = dataState
					this.emit(clonedData)
				}
				this.emit(val)
				// console.log(this.data)


			},
			checkboxStateChanged(val) {
				let clonedData = this.modelValue

				const rec = (items, state) => {
					items.forEach(child => {
						child.state.isChecked = state
						if (child.children) {
							child.children = rec(child.children, state)
						}
					})
					return items
				}

				if (clonedData.children) {
					clonedData.children = rec(clonedData.children, val)
				}

				clonedData.state.isChecked = val

				this.emit(clonedData)
			},
			emit(data) {
				this.$emit('update:modelValue', data)
			},
			toggleCollapse() {
				if (this.modelValue.children)
					this.$refs.treeList.classList.toggle('collapse')
			},
		}
	}
</script>

<style>
	.tree-list {
		display: none;
	}
	.collapse {
		display: block;
	}
</style>