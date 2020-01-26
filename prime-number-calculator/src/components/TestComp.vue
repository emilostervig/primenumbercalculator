<template>
	<section class="testcomp">
	  	<h1 class="test-message">Hello from test component</h1>
	  	<h3>{{ msg }}</h3>
	</section>

</template>


<script>

export default {
  	name: 'testcomp',
	data () {
		return {
			msg: 'This is a message from component data'
		}
	},
	methods: {
		created () {
			console.log('hello');
		}
	}		

}

</script>

<style lang="scss">

	.test-message{
		color: red;
	}

</style>