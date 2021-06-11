<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		globalData: {
			// URL: 'https://dev.test.lexiangjincheng.com',
			URL: 'http://192.168.31.70:7002',
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "./common/app.css";
</style>
