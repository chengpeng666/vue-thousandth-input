<template>
	<div id="app">
		<thousandth
			v-model="value"
			@change="change"
			@blur="blur"
			@focus="focus"
			placeholder="请输入"
			:max-before-point="5"
			:max-after-point=3
			id-set="woshi"
			:show-prepend=true
		/>

		<h5><br>Value is: {{ value }}</h5>
	</div>
</template>

<script>
    import thousandth from './../components/thousandth.vue'

    export default {
        name: 'App',
        components: {
            thousandth
        },
        data() {
            return {
                value: '2243282.2443224432'
            }
        },
        methods: {
            change(obj) {
                // console.log(obj, 'change');
            },
            blur(obj) {
                // console.log(obj, 'blur');
            },
            focus(obj) {
                // console.log(obj, 'focus');
            }
        }
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
