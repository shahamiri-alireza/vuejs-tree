<template>
	<ul class="main-tree" :dir="settings.treeDirection">
		<TreeItem v-for="item in modelValue" :level="0" :checkboxOptions="settings.checkboxOptions" :fields="settings.customFields" :key="item[settings.customFields.text]" :modelValue="item" @update:modelValue="item" @change="val => emitChanges(val)" ref="treeItem" />
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

			getField(level) {

				const isCustomFieldArray = _.isArray(this.settings.customFields)

				let fields
				if (isCustomFieldArray)
					fields = this.settings.customFields[level] ? this.settings.customFields[level] : this.settings.customFields[0]

				else
					fields = this.settings.customFields

				return fields
			},

			changeItemStateByKeyValue(key, value, state) {

				var found = false

				const recursiveStateChanger = (treeItems, level = 0) => {

					let fields = this.getField(level)


					for (let item of treeItems) {
						if (item[key] == value) {
							item[fields.state].isChecked = state
							found = true
							break
						}

						if (item[fields.children]?.length) {
							recursiveStateChanger(item[fields.children], level + 1)
							if (found) {
								break
							}
						}
					}

				}

				recursiveStateChanger(this.modelValue)
			},

			emitChanges(val) {
				this.$emit('change', val)
			},
			treeInit() {
				this.addOrCheckStateFieldtoData(this.modelValue, false)
			},
			addOrCheckStateFieldtoData(dataList, parentState = false, level = 0) {

				let fields = this.getField(level)

				for (let i = 0; i < dataList.length; i++) {
					if (parentState == true) {
						dataList[i][fields.state] = { isChecked: true }
					}
					else if (!dataList[i][fields.state]) {
						dataList[i][fields.state] = { isChecked: false }
					}
					else {
						if (dataList[i][fields.children]) {
							const chilrenState = this.getLastLevelItems(dataList[i][fields.children]).every(child => child[this.settings.customFields.state].isChecked === true)
							dataList[i][fields.state] = { isChecked: chilrenState }
						}
					}

					let nextLevel = level + 1
					if (dataList[i][fields.children]?.length > 0)
						this.addOrCheckStateFieldtoData(dataList[i][fields.children], dataList[i][fields.state].isChecked, nextLevel)
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
					// customFields: [
					// 	{
					// 		text: 'text',
					// 		children: 'children',
					// 		state: 'state'
					// 	},
					// 	{
					// 		text: 'esm',
					// 		children: 'items',
					// 		state: 'state'
					// 	},
					// 	{
					// 		text: 'matn',
					// 		children: 'bache',
					// 		state: 'state'
					// 	},
					// ],
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
