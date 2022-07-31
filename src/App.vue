<template>
	<Tree :data="testData2" />
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
				],
				testData2: [
					{
						name: 'حسابداری',
						children: [
							{
								name: 'سند',
								children: [
									{
										name: 'صدور سند'
									},
									{
										name: 'لیست اسناد'
									},
								]
							},
							{
								name: 'درخت حسابها'
							},
							{
								name: 'حسابهای شناور'
							}
						]
					},
					{
						name: 'خزانه داری'
					}
				]
			}
		},
	}
</script>
