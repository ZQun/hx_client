<template>
	<view class="container">
		<view class="tui-order-header">
			<image :src="webURL+'img_detail_bg.png'" mode="widthFix" class="tui-img-bg"></image>
			<view class="tui-header-content">
				<view>
					<view class="tui-status-text">{{orderStatus}}</view>
					<!-- <view class="tui-reason"><text class="tui-reason-text">{{getReason(status)}}</text>
						<tui-countdown :time="1800" color="rgba(254,254,254,0.75)" colonColor="rgba(254,254,254,0.75)" bcolor="transparent"
						bgcolor="transparent" v-if="status===1"></tui-countdown>
					</view> -->
				</view>
				<image :src="getImg(status)" class="tui-status-img" mode="widthFix"></image>
			</view>
		</view>
		<tui-list-cell v-if="orderInfo.status == 3 | 4 | 5 && orderInfo.express_name !== null" :arrow="true" bgcolor="#fefefe">
			<view class="tui-flex-box">
				<!-- <image :src="webURL+'img_order_logistics3x.png'" class="tui-icon-img"></image> -->
				<view class="tui-logistics">
					<view class="tui-logistics-text">{{orderInfo.express_name}}</view>
					<view class="tui-logistics-text">快递单号: {{orderInfo.express_number}}</view>
					<!-- <view class="tui-logistics-time">2019-06-03 12:02</view> -->
				</view>
			</view>
		</tui-list-cell>
		<tui-list-cell v-if="orderInfo.hx_user_address !== null" :last="true" :hover="false">
			<view class="tui-flex-box">
				<image :src="webURL+'img_order_address3x.png'" class="tui-icon-img"></image>
				<view class="tui-addr">
					<view class="tui-addr-userinfo">{{orderInfo.hx_user_address.name}}<text class="tui-addr-tel">{{orderInfo.hx_user_address.phone}}</text></view>
					<view class="tui-addr-text">{{orderInfo.hx_user_address.province + orderInfo.hx_user_address.city + orderInfo.hx_user_address.county + orderInfo.hx_user_address.address}}</view>
				</view>
			</view>
		</tui-list-cell>

		<view class="tui-order-item">
			<tui-list-cell :hover="false" :lineLeft="false">
				<view class="tui-goods-title">
					商品信息
				</view>
			</tui-list-cell>
			<block>
				<tui-list-cell padding="0">
					<view class="tui-goods-item">
						<image :src="goodsImg" class="tui-goods-img"></image>
						<view class="tui-goods-center">
							<view class="tui-goods-name">{{goodsTitle}}</view>
							<!-- <view class="tui-goods-attr">黑色，50ml</view> -->
						</view>
						<view class="tui-price-right">
							<view>{{goodsPrice}}</view>
							<view>x{{orderInfo.size}}</view>
						</view>
					</view>
				</tui-list-cell>
			</block>
			<view class="tui-goods-info">
				<!-- <view class="tui-price-flex tui-size24">
					<view>商品总额</view>
					<view>￥1192.00</view>
				</view> -->
				<!-- <view class="tui-price-flex  tui-size24">
					<view>优惠券</view>
					<view>￥0.00</view>
				</view> -->
				<view class="tui-price-flex  tui-size24">
					<view>配送费</view>
					<view>{{goodsExpressPrice}}</view>
				</view>
				<!-- <view class="tui-price-flex tui-size32 tui-pbtm20">
					<view class="tui-flex-shrink">合计</view>
					<view class="tui-goods-price">
						<view class="tui-size-24">￥</view>
						<view class="tui-price-large">1192</view>
						<view class="tui-size-24">.00</view>
					</view>
				</view> -->
				<view class="tui-price-flex tui-size32">
					<view class="tui-flex-shrink">实付款</view>
					<view class="tui-goods-price tui-primary-color">
						<view class="tui-size-24">￥</view>
						<view class="tui-price-large">{{goodsCountPrice}}</view>
						<!-- <view class="tui-size-24">.00</view> -->
					</view>
				</view>
			</view>
		</view>

		<view class="tui-order-info">
			<tui-list-cell :hover="false">
				<view class="tui-order-title">
					订单信息
				</view>
			</tui-list-cell>
			<view class="tui-order-content">
				<view class="tui-order-flex">
					<view class="tui-item-title">订单号:</view>
					<view class="tui-item-content">{{orderInfo.order_number}}</view>
				</view>
				<!-- <view class="tui-order-flex">
					<view class="tui-item-title">物流单号:</view>
					<view class="tui-item-content">33655511251265578556</view>
				</view> -->
				<view class="tui-order-flex">
					<view class="tui-item-title">创建时间:</view>
					<view class="tui-item-content">{{orderInfo.created_at}}</view>
				</view>
				<!-- <view class="tui-order-flex">
					<view class="tui-item-title">付款时间:</view>
					<view class="tui-item-content">2019-05-26 10:44</view>
				</view> -->
				<!-- <view class="tui-order-flex">
					<view class="tui-item-title">发货时间:</view>
					<view class="tui-item-content">2019-05-27 10:20</view>
				</view> -->
				<!-- <view class="tui-order-flex">
					<view class="tui-item-title">配送方式:</view>
					<view class="tui-item-content">包邮</view>
				</view> -->
				<view class="tui-order-flex">
					<view class="tui-item-title">订单备注:</view>
					<view class="tui-item-content">{{orderInfo.comment ? orderInfo.comment : '暂无'}}</view>
				</view>
			</view>
		</view>
		<view class="tui-safe-area"></view>
		<!-- <view class="tui-tabbar tui-order-btn">
			<view class="tui-btn-mr">
				<tui-button type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">删除订单</tui-button>
			</view>
			<view class="tui-btn-mr">
				<tui-button type="danger" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">立即支付</tui-button>
			</view>
		</view> -->
	</view>
</template>

<script>
	import tuiIcon from '@/components/icon/icon'
	import tuiButton from "@/components/extend/button/button"
	import tuiCountdown from "@/components/countdown/countdown"
	import tuiListCell from "@/components/list-cell/list-cell"
	export default {
		components: {
			tuiIcon,
			tuiButton,
			tuiCountdown,
			tuiListCell
		},
		data() {
			return {
				webURL: "https://www.thorui.cn/wx/static/images/mall/order/",
				//1-待付款 2-付款成功 3-待收货 4-订单已完成 5-交易关闭
				status: 1,
				id: 0,
				orderInfo: {
					type: 1,
					size: 0,
					points: 0,
					price: 0,
					order_number: 0,
					created_at: '',
					comment: '',
					hx_goods: {
						title: '',
						img_url: '[""]',
						price: 0
					},
					hx_exercise_bargain: {
						hx_goods: { title: '', img_url: '[""]', price: 0 }
					},
					hx_exercise_fight: {
						hx_goods: { title: '', img_url: '[""]', price: 0 }
					},
					hx_user_member: {
						name: '', price: 0
					},
					hx_exercise_time_limit: {
						hx_goods: { title: '', img_url: '[""]', price: 0 }
					},
					hx_exercise_points: {
						hx_goods: { title: '', img_url: '[""]', points: 0 }
					},
					hx_merchant: {
						title: ''
					}
				}
			}
		},
		onLoad(options) {
			console.log('---->options', options)
			this.id = Number(options.id)
			if(this.id) {
				this.getOrderInfo(this.id)
			}
		},
		computed: {
			orderStatus: function() {
				switch(this.orderInfo.type) {
					case 1: { return '商品订单' }
					case 2: { return '会员订单' }
					case 3: { return '砍价订单' }
					case 4: { return '拼团订单' }
					case 5: { return '购买会员' }
					case 6: { return '限时秒杀' }
					case 7: { return '阳光币兑换' }
					case 8: { return '商户入驻' }
					case 9: { return '保证金' }
				}
			},
			goodsImg: function() {
				if(this.orderInfo.type == 5) return this.state.img_url
				if(this.orderInfo.type == 8) return this.state.logo_url
				if(this.orderInfo.type == 9) return this.state.logo_url
				return JSON.parse(this.state.img_url)[0]
			},
			goodsTitle: function() {
				if(this.orderInfo.type == 5) return this.state.name;
				return this.state.title;
			},
			goodsPrice: function() {
				if(this.orderInfo.type == 7) return '阳光币' + this.orderInfo.points;
				if(this.orderInfo.type == 8) return this.orderInfo.price / 100;
				if(this.orderInfo.type == 9) return this.orderInfo.price / 100;
				return '¥' + this.state.price / 100;
			},
			goodsCountPrice: function() {
				if(this.orderInfo.type == 7) return this.orderInfo.points;
				return this.orderInfo.price * this.orderInfo.size / 100;
			},
			goodsExpressPrice: function() {
				if(this.orderInfo.type == 5) return 0;
				if(this.orderInfo.type == 7) return 0;
				return '¥' + this.orderInfo.express_price / 100;
			},
			state: function() {
				switch(this.orderInfo.type) {
					case 1: { return this.orderInfo.hx_goods }
					case 2: { return this.orderInfo.hx_goods }
					case 3: { return this.orderInfo.hx_exercise_bargain.hx_goods }
					case 4: { return this.orderInfo.hx_exercise_fight.hx_goods }
					case 5: { return this.orderInfo.hx_user_member }
					case 6: { return this.orderInfo.hx_exercise_time_limit.hx_goods }
					case 7: { return this.orderInfo.hx_exercise_points.hx_goods }
					case 8: { return this.orderInfo.hx_merchant }
					case 9: { return this.orderInfo.hx_merchant }
				}
			}
		},
		methods: {
			getImg: function(status) {
				return this.webURL + ["img_order_payment3x.png", "img_order_send3x.png", "img_order_received3x.png",
					"img_order_signed3x.png", "img_order_closed3x.png"
				][status - 1]
			},
			getStatusText: function(status) {
				return ["等待您付款", "付款成功", "待收货", "订单已完成", "交易关闭"][status - 1]
			},
			getReason: function(status) {
				return ["剩余时间", "等待卖家发货", "还剩X天XX小时自动确认", "", "超时未付款，订单自动取消"][status - 1]
			},
			getOrderInfo(id) {
				console.log('->')
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientOrder/getUserOrderInfo?id=${id}`,
							method: 'GET',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							success: (res) => {
								// SUCCESS
								this.orderInfo = res.data;
							}
						})
					},
					fail: (fail) => {
						// 页面提示登录
						uni.showToast({ title: '登录后操作！', duration: 1000 });
						setTimeout(() => {
							uni.navigateTo({
								url: '/pages/Ahuixiang/login/login',
								animationType: 'pop-in',
								animationDuration: 200
							});
						}, 1000)
					}
				});
			}
		}
	}
</script>

<style>
	.container {
		padding-bottom: 118rpx;
	}

	.tui-order-header {
		width: 100%;
		height: 160rpx;
		position: relative;
	}

	.tui-img-bg {
		width: 100%;
		height: 160rpx;
	}

	.tui-header-content {
		width: 100%;
		height: 160rpx;
		position: absolute;
		z-index: 10;
		left: 0;
		top: 0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 70rpx;
		box-sizing: border-box;
	}

	.tui-status-text {
		font-size: 36rpx;
		line-height: 36rpx;
		color: #FEFEFE;
	}

	.tui-reason {
		font-size: 24rpx;
		line-height: 24rpx;
		color: rgba(254, 254, 254, 0.75);
		padding-top: 15rpx;
		display: flex;
		align-items: center;
	}

	.tui-reason-text {
		padding-right: 12rpx;
	}

	.tui-status-img {
		width: 80rpx;
		height: 80rpx;
		display: block;
	}

	.tui-flex-box {
		width: 100%;
		display: flex;
		align-items: center;
	}

	.tui-icon-img {
		width: 44rpx;
		height: 44rpx;
		flex-shrink: 0;
	}

	.tui-logistics {
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding: 0 24rpx 0 20rpx;
		box-sizing: border-box;
	}

	.tui-logistics-text {
		font-size: 28rpx;
		line-height: 32rpx;
	}

	.tui-logistics-time {
		font-size: 24rpx;
		line-height: 24rpx;
		padding-top: 16rpx;
		color: #666;
	}

	.tui-addr {
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding-left: 20rpx;
		box-sizing: border-box;
	}

	.tui-addr-userinfo {
		font-size: 30rpx;
		line-height: 30rpx;
		font-weight: bold;
	}

	.tui-addr-text {
		font-size: 24rpx;
		line-height: 30rpx;
		padding-top: 16rpx;
	}

	.tui-addr-tel {
		padding-left: 40rpx;
	}

	.tui-order-item {
		margin-top: 20rpx;
		border-radius: 10rpx;
		overflow: hidden;
	}

	.tui-goods-title {
		width: 100%;
		font-size: 28rpx;
		line-height: 28rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}


	.tui-goods-item {
		width: 100%;
		padding: 20rpx 30rpx;
		box-sizing: border-box;
		display: flex;
		justify-content: space-between;
	}

	.tui-goods-img {
		width: 180rpx;
		height: 180rpx;
		display: block;
		flex-shrink: 0;
	}

	.tui-goods-center {
		flex: 1;
		padding: 20rpx 8rpx;
		box-sizing: border-box;
	}

	.tui-goods-name {
		max-width: 310rpx;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
		font-size: 26rpx;
		line-height: 32rpx;
	}

	.tui-goods-attr {
		font-size: 22rpx;
		color: #888888;
		line-height: 32rpx;
		padding-top: 20rpx;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.tui-price-right {
		text-align: right;
		font-size: 24rpx;
		color: #888888;
		line-height: 30rpx;
		padding-top: 20rpx;
	}

	.tui-color-red {
		color: #E41F19;
		padding-right: 30rpx;
	}

	.tui-goods-price {
		width: 100%;
		display: flex;
		align-items: flex-end;
		justify-content: flex-end;
		font-size: 24rpx;
	}

	.tui-size-24 {
		font-size: 24rpx;
		line-height: 24rpx;
	}

	.tui-price-large {
		font-size: 32rpx;
		line-height: 30rpx;
	}

	.tui-goods-info {
		width: 100%;
		padding: 30rpx;
		box-sizing: border-box;
		background: #fff;
	}

	.tui-price-flex {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.tui-size24 {
		padding-bottom: 20rpx;
		font-size: 24rpx;
		line-height: 24rpx;
		color: #888;
	}

	.tui-size32 {
		font-size: 32rpx;
		line-height: 32rpx;
		font-weight: 500;
	}

	.tui-pbtm20 {
		padding-bottom: 20rpx;
	}

	.tui-flex-shrink {
		flex-shrink: 0;
	}

	.tui-primary-color {
		color: #EB0909;
	}

	.tui-order-info {
		margin-top: 20rpx;
	}

	.tui-order-title {
		position: relative;
		font-size: 28rpx;
		line-height: 28rpx;
		padding-left: 12rpx;
		box-sizing: border-box;
	}

	.tui-order-title::before {
		content: '';
		position: absolute;
		left: 0;
		top: 0;
		border-left: 4rpx solid #EB0909;
		height: 100%;
	}

	.tui-order-content {
		width: 100%;
		padding: 24rpx 30rpx;
		box-sizing: border-box;
		background: #fff;
		font-size: 24rpx;
		line-height: 30rpx;
	}

	.tui-order-flex {
		display: flex;
		padding-top: 18rpx;
	}

	.tui-order-flex:first-child {
		padding-top: 0
	}

	.tui-item-title {
		width: 132rpx;
		flex-shrink: 0;
	}

	.tui-item-content {
		color: #666;
	}

	.tui-safe-area {
		height: 1rpx;
		padding-bottom: env(safe-area-inset-bottom);
	}

	.tui-tabbar {
		width: 100%;
		height: 98rpx;
		background: #fff;
		position: fixed;
		left: 0;
		bottom: 0;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		font-size: 26rpx;
		box-shadow: 0 0 1px rgba(0, 0, 0, .3);
		padding-bottom: env(safe-area-inset-bottom);
		z-index: 999;
	}
	
	.tui-btn-mr {
		margin-right: 30rpx;
	}
</style>
