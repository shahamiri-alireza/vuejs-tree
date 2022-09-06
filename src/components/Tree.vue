<template>
	<ul class="main-tree" :dir="settings.treeDirection">
		<TreeItem v-for="item in modelValue" :checkboxOptions="settings.checkboxOptions" :fields="settings.customFields" :key="item[settings.customFields.text]" :modelValue="item" @update:modelValue="item" @change="val => emitChanges(val)" />
	</ul>
</template>

<script>
	import TreeItem from './TreeItem.vue'
	import _ from 'lodash'

	export default {
		name: 'Tree',
		components: {
			TreeItem,
		},
		props: {
			modelValue: { type: Array, default: [] },
			options: { type: Object }
		},
		emits: ['change'],
		watch: {
			options: {
				deep: true,
				immediate: true,
				handler(val) {
					this.settings = _.merge(this.settings, val)
				}
			},
		},
		created() {
			this.treeInit()
		},
		methods: {
			emitChanges(val) {
				this.$emit('change', val)
			},
			treeInit() {
				this.addOrCheckStateFieldtoData(this.modelValue, false)
			},
			addOrCheckStateFieldtoData(dataList, parentState = false) {
				for (let i = 0; i < dataList.length; i++) {
					if (parentState == true) {
						dataList[i][this.settings.customFields.state] = { isChecked: true }
					}
					else if (!dataList[i][this.settings.customFields.state]) {
						dataList[i][this.settings.customFields.state] = { isChecked: false }
					}
					else {
						if (dataList[i][this.settings.customFields.children]) {
							const chilrenState = this.getLastLevelItems(dataList[i][this.settings.customFields.children]).every(child => child[this.settings.customFields.state].isChecked === true)
							dataList[i][this.settings.customFields.state] = { isChecked: chilrenState }
						}
					}


					if (dataList[i][this.settings.customFields.children]?.length > 0)
						this.addOrCheckStateFieldtoData(dataList[i][this.settings.customFields.children], dataList[i][this.settings.customFields.state].isChecked)
				}
			},
			getLastLevelItems(items) {
				let lastLevelItems = []

				const getComputeds = (item) => {
					if (item[this.settings.customFields.children]?.length) {
						item[this.settings.customFields.children].forEach(child => getComputeds(child))
					} else {
						lastLevelItems.push(item)
					}
				}

				for (let i = 0; i < items.length; i++) {
					getComputeds(items[i])
				}

				return lastLevelItems
			},
			getLastLevelChecked() {
				let result = _.cloneDeep(this.modelValue)

				let computed = []

				const getComputeds = (item) => {
					if (item[this.settings.customFields.children]?.length) {
						item[this.settings.customFields.children].forEach(child => getComputeds(child))
					} else {
						if (item[this.settings.customFields.state].isChecked) {
							computed.push(item)
						}
					}
				}

				for (let i = 0; i < result.length; i++) {
					getComputeds(result[i])
				}

				return computed
			},
		},
		data() {
			return {
				settings: {
					treeDirection: 'rtl', // ltr | rtl
					customFields: {
						text: 'text',
						children: 'children',
						state: 'state'
					},
					checkboxOptions: {
						checkedColor: '#466eb5',
						unCheckedColor: 'white'
					}
				}
			}
		},
	}
</script>

<style scoped>
	ul {
		list-style-type: none;
	}
	li {
		list-style: none;
		margin: 0px 10px;
	}
	a {
		color: #42b983;
	}
</style>
