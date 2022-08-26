<template>
	<div class="checkbox" :class="{'checked':state !== 'unchecked'}" @click="checkboxClicked">
		<img v-if="state === 'checked'" src="../assets/icons/checked.svg" alt="">
		<img v-else-if="state === 'halfChecked'" src="../assets/icons/halfChecked.svg" alt="">
	</div>
</template>

<script>
	import _ from 'lodash'
	export default {
		name: 'Checkbox',
		props: {
			modelValue: {},
			options: {},
			isHalf: { default: false, type: Boolean }
		},
		data() {
			return {
				state: 'unchecked', // checked unchecked halfChecked
			}
		},
		watch: {
			modelValue: {
				immediate: true,
				handler(val) {
					this.state = val
				}

			}
		},
		created() {
			if (this.modelValue)
				this.state = this.modelValue
		},
		mounted() {
		},
		methods: {
			checkboxClicked() {
				switch (this.state) {
					case 'checked':
						this.state = 'unchecked'
						break
					case 'unchecked':
					case 'halfChecked':
						this.state = 'checked'
						break
				}

				this.$emit('update:modelValue', this.state === 'checked' ? true : false)
			}
		}
	}
</script>

<style>
	.checkbox {
		width: 20px;
		height: 20px;
		border: 1px solid #dadada;
		border-radius: 4px;
		padding: 2px;
		display: inline-block;
		margin: 0px 4px;
		background-color: white;
	}

	.checked {
		background-color: rgb(70, 70, 255);
	}

	.checkbox img {
		width: 100%;
		height: 100%;
	}
</style>