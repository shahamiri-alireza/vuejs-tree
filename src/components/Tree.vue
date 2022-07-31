<template>
	<ul class="main-tree" :dir="settings.treeDirection">
		<TreeItem v-for="item in data" :key="item.name" :data="item" />
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
			data: { type: Array, default: [] },
			options: { type: Object }
		},
		watch:{
			options:{
				deep:true,
				immediate:true,
				handler(val){
					this.settings = _.merge(this.settings, val)
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
