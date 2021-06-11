<template>
	<view>
		<web-view :webview-styles="webviewStyles" @message="message" :src="url"></web-view>
	</view>
</template>

<script>
export default {
	components: {

	},
	data() {
		return {
			webviewStyles: {
				progress: false
			},
			// url: `http://fc.lexiangjincheng.com/index.html?data=`,
			url: `https://fc.y0356.com/index.html?data=`,
			// url: `http://192.168.31.70:8080?data=`,
			token: ''
		}
	},
	onLoad(options) {
		this.token = options.token;
		console.log('->token', this.token)
		console.log('->token', encodeURIComponent(this.token))
		this.url += encodeURIComponent(this.token)
	},
	methods: {
		message(event){
            console.log('return.event', event.detail.data);
        }
	}
}
</script>

<style>

</style>
