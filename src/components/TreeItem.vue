<template>
	<li class="tree-item">
		<span @click="toggleCollapse">{{ data.name }}</span>
		<ul class="tree-list" v-if="data.children" ref="treeList">
			<TreeItem v-for="item in data.children" :key="item.name" :data="item" />
		</ul>
	</li>
</template>
<script>
	export default {
		name: 'TreeItem',
		components: {
		},
		props: {
			data: {},
			options: { type: Object }
		},
		methods: {
			toggleCollapse() {
				if (this.data.children)
					this.$refs.treeList.classList.toggle('collapse')
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