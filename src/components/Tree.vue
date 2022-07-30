<template>
    <ul class="main-tree">
      <TreeItem v-for="item in data" :key="item.name" :data="item" />
    </ul>
</template>

<script>
import TreeItem from './TreeItem.vue'
	export default {
		name: 'Tree',
    components:{
      TreeItem,
    },
		props: {
			data: { type: Array, default: [] },
			options: { type: Object }
		},
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	ul {
		list-style-type: none;
		/* padding: 0; */
	}
	li {
		/* display: inline-block; */
		margin: 10px 10px;
	}
	a {
		color: #42b983;
	}
</style>
