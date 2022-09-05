<template>
	<div class="checkbox" :class="{'checked':state !== 'unchecked'}" :style="{'background-color': state !== 'unchecked' ? settings.checkedColor : settings.unCheckedColor}" @click="checkboxClicked">
		<img class="checkbox-icon" v-show="state === 'checked'" src="../assets/icons/checked.svg">
		<img class="checkbox-icon" v-show="state === 'halfChecked'" src="../assets/icons/halfChecked.svg">
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
				settings: {
					checkedColor: '#4646ff',
					unCheckedColor: 'white'
				},
				state: 'unchecked', // checked unchecked halfChecked
			}
		},
		watch: {
			modelValue: {
				immediate: true,
				handler(val) {
					this.state = val
				}
			},
			options: {
				immediate: true,
				deep: true,
				handler(val) {
					this.settings = _.merge(this.settings, val)
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
		transition: background-color 0.2s ease-in-out;
	}

	.checkbox img {
		width: 100%;
		height: 100%;
	}

.checkbox-icon{
	animation: checbkoxAnimation 0.2s ease-in-out forwards;
}


@keyframes checbkoxAnimation{
			from {
			opacity: 0;
			transform: rotate(20deg);
		}
		to {
			opacity: 1;
			transform: rotate(0deg);
		}
}
</style>