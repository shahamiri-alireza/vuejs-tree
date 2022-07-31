<template>
	<li class="tree-item">
		<Checkbox :value="checkboxState" @click="checkboxClicked" />
		<span @click="toggleCollapse">{{ data.name }}</span>
		<ul class="tree-list" v-if="data.children" ref="treeList">
			<TreeItem v-for="item in data.children" :key="item.name" :data="item" />
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
			data: {},
			options: { type: Object }
		},
		data() {
			return {
				state: { isChecked: false, isHalfChecked: false },
				checkboxState: 'unchecked',
			}
		},
		methods: {
			toggleCollapse() {
				if (this.data.children)
					this.$refs.treeList.classList.toggle('collapse')
			},
			updateCheckboxState() {
				if (this.state.isChecked) {
					return 'checked'
				}
				else if (this.state.isChecked === false) {
					if (this.state.isHalfChecked) {
						return 'halfChecked'
					}
					else {
						return 'unchecked'
					}
				}
			},
			checkboxClicked() {
				const rec = (item, state) => {
					// if (!item.children) return
					item.children.forEach(child => {
						child.state = state
						if (child.children) {
							rec(child, state)
						}
					})
					return item
				}
				// console.log(this.data)
				// return
				if (this.state.isChecked) {
					this.state.isChecked = false
				}
				else {
					this.state.isChecked = true
					// this.data.children.map(child => child.state.isChecked = true)
				}
				if (this.data.children) {
					this.data = rec(this.data, this.state)
					// this.data.state = this.state
				}
				// this.state.isHalfChecked = false
				// this.data.state = this.state

				this.checkboxState = this.updateCheckboxState()
				// console.log(this.updateCheckboxState())
			},
			shareNewState(state) {
				if (!this.data.children) return

			}
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