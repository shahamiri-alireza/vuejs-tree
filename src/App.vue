<template>
	<Tree :data="testData" />
</template>

<script>
	import Tree from './components/Tree.vue'

	export default {
		name: 'App',
		components: {
			Tree,
		},
		data() {
			return {
				testData: [
					{
						name: 'P1',
						children: [
							{
								name: 'C11',
								children: [
									{
										name: 'C21'
									},
									{
										name: 'C22'
									},
								]
							},
							{
								name: 'C12'
							},
							{
								name: 'C13'
							}
						]
					},
					{
						name: 'P2'
					}
				]
			}
		},
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
