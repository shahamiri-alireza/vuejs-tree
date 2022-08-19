<template>
	<ul class="main-tree" :dir="settings.treeDirection">
		<TreeItem v-for="item in modelValue" :key="item.na" :modelValue="item" @update:modelValue="item = $event" />
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
						dataList[i].state = { isChecked: true }
					}
					else if (!dataList[i].state) {
						dataList[i].state = { isChecked: false }
					}


					if (dataList[i].children?.length > 0)
						this.addOrCheckStateFieldtoData(dataList[i].children, dataList[i].state.isChecked)
				}
			}
		},
		data() {
			return {
				settings: {
					treeDirection: 'rtl', // ltr | rtl
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
