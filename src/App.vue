<template>
	<Tree :modelValue="testData2" :options="testOptions" />
	<button @click="test1()">Salam</button>
</template>

<script>
	import Tree from './components/Tree.vue'

	export default {
		text: 'App',
		components: {
			Tree,
		},
		data() {
			return {
				testOptions: {
					customFields: {
						text: 'text'
					}
				},
				testData: [
					{
						text: 'P1',
						children: [
							{
								text: 'C11',
								children: [
									{
										text: 'C21',
									},
									{
										text: 'C22',
									},
								]
							},
							{
								text: 'C12',
							},
							{
								text: 'C13',
							}
						]
					},
					{
						text: 'P2',
					}
				],
				testData2: [
					{
						text: 'حسابداری',
						children: [
							{
								text: 'سند',
								children: [
									{
										text: 'صدور سند',
									},
									{
										text: 'لیست اسناد',
									},
								]
							},
							{
								text: 'درخت حسابها',
							},
							{
								text: 'حسابهای شناور',
							}
						]
					},
					{
						text: 'خزانه داری',
					}
				]
			}
		},
		methods: {
			test1() {
				console.log(this.testData2)
			}
		},
	}
</script>
