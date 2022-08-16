<template>
	<li class="tree-item">
		<Checkbox :value="checkboxState" @checkboxStateChanged="val => checkboxStateChanged(val)" />
		<span @click="toggleCollapse">{{ data.name }}</span>
		<ul class="tree-list" v-if="data.children" ref="treeList">
			<TreeItem v-for="item in data.children" :key="item.name" :data="item" @dataChanged="val => treeItemDataChanged(val)" />
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
				// checkboxState: 'unchecked',
			}
		},
		computed: {
			checkboxState() {
				if (this.data.state.isChecked) {
					return 'checked'
				}
				else if (this.data.state.isChecked === false) {
					if (this.data.state.isHalfChecked) {
						return 'halfChecked'
					}
					else {
						return 'unchecked'
					}
				}
			},
		},
		methods: {
			treeItemDataChanged(val){
				if (val.state.isChecked === false){
					this.data.state.isChecked = false
				}
				

				this.emit(val)
			},
			checkboxStateChanged(val) {
				let clonedData = this.data

				const rec = (items, state) => {
					// if (!item.children) return
					items.forEach(child => {
						child.state = state
						if (child.children) {
							child.children = rec(child.children, state)
						}
					})
					return items
				}

				if (clonedData.children) {
					clonedData.children = rec(clonedData.children, { isChecked: val })
				}

				clonedData.state.isChecked = true

				this.emit(clonedData)

			},
			emit(data) {
				this.$emit('dataChanged', data)
			},
			toggleCollapse() {
				if (this.data.children)
					this.$refs.treeList.classList.toggle('collapse')
			},
			checkboxClicked() {
				return
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
				// let a = this.data
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
					rec(this.data, this.state)
					// this.data.state = this.state
				}
				this.$emit('dataChanged',)
				// this.state.isHalfChecked = false
				// this.data.state = this.state

				this.checkboxState = this.updateCheckboxState()
				// console.log(this.updateCheckboxState())
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