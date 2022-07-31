<template>
	<div class="checkbox" :class="{'checked':state !== 'unchecked'}" @click="emitClick">
		<img v-if="state !== 'unchecked'" :src="require(`../assets/icons/${state}.svg`)" alt="">
	</div>
</template>

<script>
	import _ from 'lodash'
	export default {
		name: 'Checkbox',
		props: {
			value: {},
			options: {},
			isHalf: { default: false, type: Boolean }
		},
		data() {
			return {
				state: 'unchecked', // checked unChecked halfChecked
			}
		},
		watch: {
      value:{
        immediate:true,
        handler(val){
          console.log(val)
          this.state = val
        }

      }
		},
		created() {
			if (this.value)
				this.state = this.value
		},
		mounted() {
		},
		methods: {
      emitClick(){
        // this.$emit('click')
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