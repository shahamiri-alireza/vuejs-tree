<template>
	<ul class="main-tree" :dir="settings.treeDirection">
		<TreeItem v-for="item in modelValue" :checkboxOptions="settings.checkboxOptions" :fields="settings.customFields" :key="item[settings.customFields.text]" :modelValue="item" @update:modelValue="item = $event" />
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
		watch: {
			options: {
				deep: true,
				immediate: true,
				handler(val) {
					this.settings = _.merge(this.settings, val)
				}
			}
		},
		created() {
			this.treeInit()
		},
		methods: {
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


					if (dataList[i][this.settings.customFields.children]?.length > 0)
						this.addOrCheckStateFieldtoData(dataList[i][this.settings.customFields.children], dataList[i][this.settings.customFields.state].isChecked)
				}
			}
		},
		data() {
			return {
				settings: {
					treeDirection: 'rtl', // ltr | rtl
					customFields: {
						text: 'esm',
						children: 'children',
						state: 'state'
					},
					checkboxOptions: {
						checkedColor: '#4646ff',
						unCheckedColor: 'white'
					}
				}
			}
		},
	}
</script>

<style>
	ul {
		list-style-type: none;
	}
	li {
		list-style: none;
		margin: 10px 10px;
	}
	a {
		color: #42b983;
	}
</style>
