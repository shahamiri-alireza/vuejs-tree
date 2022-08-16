<template>
	<Tree :data="testData2" />
	<button @click="test1()">Salam</button>
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
						state: { isChecked: false },
						children: [
							{
								name: 'C11',
								state: { isChecked: false },
								children: [
									{
										name: 'C21',
										state: { isChecked: false },
									},
									{
										name: 'C22',
										state: { isChecked: false },
									},
								]
							},
							{
								name: 'C12',
								state: { isChecked: false },
							},
							{
								name: 'C13',
								state: { isChecked: false },
							}
						]
					},
					{
						name: 'P2',
						state: { isChecked: false },
					}
				],
				testData2: [
					{
						name: 'حسابداری',
						state: { isChecked: false },
						children: [
							{
								name: 'سند',
								state: { isChecked: false },
								children: [
									{
										name: 'صدور سند',
										state: { isChecked: false },
									},
									{
										name: 'لیست اسناد',
										state: { isChecked: false },
									},
								]
							},
							{
								name: 'درخت حسابها',
								state: { isChecked: false },
							},
							{
								name: 'حسابهای شناور',
								state: { isChecked: false },
							}
						]
					},
					{
						name: 'خزانه داری',
						state: { isChecked: false },

					}
				]
			}
		},
		methods: {
			test1(){
				console.log(this.testData2)
			}
		},
	}
</script>
