<template>
	<view class="container">
		<view class="tui-box">
			<tui-list-cell :arrow="true" :last="true" :radius="true" @tap="chooseAddr">
				<view class="tui-address">
					<view v-if="userInfo.hx_user_address.length !== 0">
						<view class="tui-userinfo">
							<text class="tui-name">{{userInfo.hx_user_address[0].name}}</text> {{userInfo.hx_user_address[0].phone}}
						</view>
						<view class="tui-addr">
							<!-- <view class="tui-addr-tag">公司</view> -->
							<text>{{userInfo.hx_user_address[0].province +'-'+ userInfo.hx_user_address[0].city +'-'+ userInfo.hx_user_address[0].county +'-'+ userInfo.hx_user_address[0].address}}</text>
						</view>
					</view>
					<view class="tui-none-addr" v-else>
						<image src="/static/images/index/map.png" class="tui-addr-img" mode="widthFix"></image>
						<text>选择收货地址</text>
					</view>
				</view>
				<view class="tui-bg-img"></view>
			</tui-list-cell>
			<view class="tui-top tui-goods-info">
				<tui-list-cell :hover="false" :lineLeft="false">
					<view class="tui-goods-title">
						商品信息
					</view>
				</tui-list-cell>
				<block v-if="goodsInfo && !cart_status">
					<tui-list-cell :hover="false" padding="0">
						<view class="tui-goods-item">
							<image :src="JSON.parse(goodsInfo.hx_goods.img_url)[0]" class="tui-goods-img"></image>
							<view class="tui-goods-center">
								<view class="tui-goods-name">{{goodsInfo.hx_goods.title}}</view>
								<!-- <view class="tui-goods-attr">黑色，50ml</view> -->
							</view>
							<view class="tui-price-right">
								<view v-if="!vip_status">阳光币:{{goodsInfo.points}}</view>
								<view v-else>VIP {{Number(goodsInfo.vip_price) / 100}}</view>
								<view>x{{size}}</view>
							</view>
						</view>
					</tui-list-cell>
				</block>
				<block v-if="cart_status" v-for="(item,index) in goodsCart" :key="index">
					<tui-list-cell :hover="false" padding="0">
						<view class="tui-goods-item">
							<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="tui-goods-img"></image>
							<view class="tui-goods-center">
								<view class="tui-goods-name">{{item.hx_goods.title}}</view>
								<!-- <view class="tui-goods-attr">黑色，50ml</view> -->
							</view>
							<view class="tui-price-right">
								<view v-if="!vip_status || !item.hx_goods.vip_price">￥{{Number(item.hx_goods.price) / 100}}</view>
								<view v-else>VIP {{Number(item.hx_goods.vip_price) / 100}}</view>
								<view>x{{item.num}}</view>
							</view>
						</view>
					</tui-list-cell>
				</block>
				<tui-list-cell v-if="cart_status" :hover="false">
					<view class="tui-padding tui-flex">
						<view>商品总额</view>
						<view>￥{{cartPrice}}</view>
					</view>
				</tui-list-cell>
				<!-- <tui-list-cell v-else :hover="false">
					<view class="tui-padding tui-flex">
						<view>商品总额</view>
						<view v-if="!vip_status">￥{{Number(goodsInfo.hx_goods.price * size) / 100}}</view>
						<view v-else>￥{{Number(goodsInfo.vip_price * size) / 100}}</view>
					</view>
				</tui-list-cell> -->
				<!-- <tui-list-cell :arrow="hasCoupon" :hover="hasCoupon" >
					<view class="tui-padding tui-flex">
						<view>优惠券</view>
						<view :class="{'tui-color-red':hasCoupon}">{{hasCoupon?"满5减1":'没有可用优惠券'}}</view>
					</view>
				</tui-list-cell>
				<tui-list-cell :hover="true" :arrow="true">
					<view class="tui-padding tui-flex">
						<view>发票</view>
						<view class="tui-invoice-text">不开发票</view>
					</view>
				</tui-list-cell> -->
				<!-- <tui-list-cell :hover="false">
					<view class="tui-padding tui-flex">
						<view>配送费</view>
						<view v-if="cart_status">￥{{expressPrice}}</view>
						<view v-else>￥{{Number(goodsInfo.hx_goods.express_price / 100)}}</view>
					</view>
				</tui-list-cell> -->
				<tui-list-cell :hover="false" :lineLeft="false" padding="0">
					<view class="tui-remark-box tui-padding tui-flex">
						<view>订单备注</view>
						<input v-model="comment" type="text" class="tui-remark" placeholder="选填: 请先和商家协商一致" placeholder-class="tui-phcolor"></input>
					</view>
				</tui-list-cell>
				<tui-list-cell :hover="false" :last="true">
					<view class="tui-padding tui-flex tui-total-flex">
						<view class="tui-flex-end tui-color-red">
							<view class="tui-black">合计： </view>
							<view class="tui-price-large">{{countPrice}}</view>
							<!-- <view class="tui-size-26">.00</view> -->
						</view>
					</view>
				</tui-list-cell>
			</view>

			<!-- <view class="tui-top">
				<tui-list-cell :last="true" :hover="insufficient" :radius="true" :arrow="insufficient">
					<view class="tui-flex">
						<view class="tui-balance">余额支付<text class="tui-gray">(￥{{ balanceUser }})</text></view>
						<switch @change="switch2Change" :checked="status" color="#30CC67" class="tui-scale-small" v-show="insufficient" />
						<view class="tui-pr-30 tui-light-dark" v-show="!insufficient">余额不足</view>
					</view>
				</tui-list-cell>
			</view> -->
		</view>
		<view class="tui-safe-area"></view>
		<view class="tui-tabbar">
			<view class="tui-flex-end tui-color-red tui-pr-20">
				<view class="tui-black">阳光币: </view>
				<!-- <view class="tui-size-26">￥</view> -->
				<view class="tui-price-large">{{countPrice}}</view>
				<!-- <view class="tui-size-26">.00</view> -->
			</view>
			<view class="tui-pr25">
				<tui-button width="200rpx" height="70rpx" type="green" shape="circle" @tap="btnPay">确认支付</tui-button>
			</view>
		</view>

	</view>
</template>

<script>
	import tuiButton from "@/components/extend/button/button"
	import tuiListCell from "@/components/list-cell/list-cell"
	import tuiBottomPopup from "@/components/bottom-popup/bottom-popup"
	export default {
		components: {
			tuiButton,
			tuiListCell,
			tuiBottomPopup
		},
		data() {
			return {
				hasCoupon: true,
				id: 0,
				size: 1,
				goodsInfo: {
					title: '',
					img_url: '[""]',
					price: 0,
					express_price: 0,
					hx_goods: {
						img_url: '[""]',
						title: '',
						express_price: 0
					},
					hx_bargain_user: [{
						price: 0
					}]
				},
				goodsCart: [],
				userInfo: {
					hx_user_address: [],
					balance: 0
				},
				status: false,
				comment: '',
				vip_status: false,
				cart_status: false,
				cart_id: null,
				type: null
			}
		},
		computed: {
			cartPrice: function () {
				let countPrice = 0;
				this.goodsCart.forEach(item => {
					if(this.vip_status && item.hx_goods.vip_price) {
						// 如果该商品没有VIP价格，则以原价计算
						countPrice = countPrice + (item.num * item.hx_goods.vip_price)
					} else {
						countPrice = countPrice + (item.num * item.hx_goods.price)
					}
				})
				return countPrice / 100;
			},
			expressPrice: function() {
				let countPrice = 0;
				this.goodsCart.forEach(item => {
					countPrice = countPrice + item.hx_goods.express_price
				})
				return countPrice / 100;
			},
			countPrice: function() {
				return Number(this.goodsInfo.points) * this.size
			},
			balanceUser: function() {
				let balance = this.userInfo.balance / 100;
				if(this.status) {
					balance = balance - this.countPrice;
					if(balance < 0) balance = 0;
				}
				return balance;
			},
			insufficient: function() {
				if(this.status) {
					return this.balanceUser >= 0 ? true : false;
				}
				return this.balanceUser > 0 ? true : false
			},
			factPayPrice: function() {
				let balance = this.userInfo.balance - (this.countPrice * 100);
				if(this.status && balance < 0) {
					return ((this.countPrice * 100) - this.userInfo.balance) / 100;
				} else {
					if(this.status) {
						return 0;
					}
					return this.countPrice;
				}
			}
		},
		onLoad(options) {
			console.log('->options', options)

			this.id = options.id ? Number(options.id) : 0;
			this.type = options.type;

			if(this.id) {
				// 查询商品信息
				this.getGoodsInfo(this.id, this.type);
			}
			// 获取用户信息
			this.getUserInfo();
			// 收货地址
			uni.$on('address', (data) => {
				this.userInfo.hx_user_address = [data];
			})
		},
		methods: {
			switch2Change(e) {
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
				this.status = e.target.value;
			},
			chooseAddr() {
				uni.navigateTo({
					url: "/pages/Ahx-user/address/address"
				})
			},
			btnPay() {
				// 提交订单   付款操作！
				if(this.userInfo.hx_user_address.length === 0) {
					this.tui.toast("您没有选择收货地址！")
				} else {
					// 整理订单信息
					let goods_id = this.id;
					let size = this.size;
					// 收货地址信息
					let address_id = this.userInfo.hx_user_address[0].id;
					// 余额支付状态
					let status = this.status;
					// 订单备注信息
					let comment = this.comment;
					// this
					let thls = this;
					// order
					let order = {
						goods_id,
						size,
					}

					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientExercise/submitPointsOrder`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									order,
									status,
									address_id,
									comment,
									type: thls.type
								},
								success: (res) => {
									if(res.data.t === 1000) {
										uni.redirectTo({
											url: "/pages/Ahuixiang/success/success"
										})
									} else {
										uni.showToast({
											title: res.data.t,
											icon: 'none',
											duration: 2000
										});
									}
								}
							})
						},
						fail: (fail) => {
							uni.navigateTo({
								url: '/pages/Ahuixiang/login/login',
								animationType: 'pop-in',
								animationDuration: 200
							});
						}
					});
				}
			},
			getCartList(id) {
				
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientGoods/GetCartList?id=${id}`,
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							success: (res) => {
								this.goodsCart = res.data !== '暂无数据' ? res.data : {};
							}
						})
					},
					fail: (fail) => {
						uni.navigateTo({
							url: '/pages/Ahuixiang/login/login',
							animationType: 'pop-in',
							animationDuration: 200
						});
					}
				});
			},
			getGoodsInfo(id, type) {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientExercise/getPointsInfo?id=${id}&type=${type}`,
							method: 'GET',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							// data: this.addForm,
							success: (res) => {
								this.goodsInfo = res.data !== '暂无数据' ? res.data : {};
							}
						})
					},
					fail: (fail) => {
						uni.navigateTo({
							url: '/pages/Ahuixiang/login/login',
							animationType: 'pop-in',
							animationDuration: 200
						});
					}
				});
				
			},
			getUserInfo() {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientUser/getUserInfo`,
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							success: (res) => {
								this.userInfo = res.data !== '暂无数据' ? res.data : {};
								if(this.cart_status) {
									this.vip_status = this.userInfo.grade === 2 ? true : false;
								}
							}
						})
					},
					fail: (fail) => {
						uni.navigateTo({
							url: '/pages/Ahuixiang/login/login',
							animationType: 'pop-in',
							animationDuration: 200
						});
					}
				});
			}
		}
	}
</script>

<style>
	.container {
		padding-bottom: 98rpx;
	}

	.tui-box {
		padding: 20rpx 0 118rpx;
		box-sizing: border-box;
	}

	.tui-address {
		min-height: 80rpx;
		padding: 10rpx 0;
		box-sizing: border-box;
		position: relative;
	}

	.tui-userinfo {
		font-size: 30rpx;
		font-weight: 500;
		line-height: 30rpx;
		padding-bottom: 12rpx;
	}

	.tui-name {
		padding-right: 40rpx;
	}

	.tui-addr {
		font-size: 24rpx;
		word-break: break-all;
		padding-right: 25rpx;
	}

	.tui-addr-tag {
		padding: 5rpx 8rpx;
		flex-shrink: 0;
		background: #EB0909;
		color: #fff;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		font-size: 25rpx;
		line-height: 25rpx;
		transform: scale(0.8);
		transform-origin: 0 center;
		border-radius: 6rpx;
	}

	.tui-bg-img {
		position: absolute;
		width: 100%;
		height: 8rpx;
		left: 0;
		bottom: 0;
		background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAL0AAAAECAMAAADszM6/AAAAOVBMVEUAAAAVqfH/fp//vWH/vWEVqfH/fp8VqfH/fp//vWEVqfH/fp8VqfH/fp//vWH/vWEVqfH/fp//vWHpE7b6AAAAEHRSTlMA6urqqlVVFRUVq6upqVZUDT4vVAAAAEZJREFUKM/t0CcOACAQRFF6r3v/w6IQJGwyDsPT882IQzQE0E3chToByjG5LwMgLZN3TQATmdypCciBya0cgOT3/h//9PgF49kd+6lTSIIAAAAASUVORK5CYII=") repeat;
	}

	.tui-top {
		margin-top: 20rpx;
		overflow: hidden;
	}

	.tui-goods-title {
		font-size: 28rpx;
		display: flex;
		align-items: center;
	}

	.tui-padding {
		box-sizing: border-box;
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

	.tui-flex {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 26rpx;
	}
	.tui-total-flex{
		justify-content: flex-end;
	}

	.tui-color-red,
	.tui-invoice-text {
		color: #E41F19;
		padding-right: 30rpx;
	}

	.tui-balance {
		font-size: 28rpx;
		font-weight: 500;
	}

	.tui-black {
		color: #222;
		line-height: 30rpx;
	}

	.tui-gray {
		color: #888888;
		font-weight: 400;
	}

	.tui-light-dark {
		color: #666;
	}

	.tui-goods-price {
		display: flex;
		align-items: center;
		padding-top: 20rpx;
	}

	.tui-size-26 {
		font-size: 26rpx;
		line-height: 26rpx;
	}

	.tui-price-large {
		font-size: 34rpx;
		line-height: 32rpx;
		font-weight: 600;
	}

	.tui-flex-end {
		display: flex;
		align-items: flex-end;
		padding-right: 0;
	}

	.tui-phcolor {
		color: #B3B3B3;
		font-size: 26rpx;
	}

	/* #ifndef H5 */
	.tui-remark-box {
		padding: 22rpx 30rpx;
	}

	/* #endif */
	/* #ifdef H5 */
	.tui-remark-box {
		padding: 26rpx 30rpx;
	}

	/* #endif */

	.tui-remark {
		flex: 1;
		font-size: 26rpx;
		padding-left: 64rpx;
	}

	.tui-scale-small {
		transform: scale(0.8);
		transform-origin: 100% center;
	}

	.tui-scale-small .wx-switch-input {
		margin: 0 !important;
	}

	/* #ifdef H5 */
	>>>uni-switch .uni-switch-input {
		margin-right: 0 !important;
	}

	/* #endif */
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

	.tui-pr-30 {
		padding-right: 30rpx;
	}

	.tui-pr-20 {
		padding-right: 20rpx;
	}

	.tui-none-addr {
		height: 80rpx;
		padding-left: 5rpx;
		display: flex;
		align-items: center;
	}

	.tui-addr-img {
		width: 36rpx;
		height: 46rpx;
		display: block;
		margin-right: 15rpx;
	}


	.tui-pr25 {
		padding-right: 25rpx;
	}

	.tui-safe-area {
		height: 1rpx;
		padding-bottom: env(safe-area-inset-bottom);
	}
</style>
