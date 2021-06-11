<template>
	<view class="container">
		<tui-tabs :tabs="tabs" :isFixed="scrollTop>=0" :currentTab="currentTab" selectedColor="#E41F19" sliderBgColor="#E41F19"
		 @change="change"></tui-tabs>
		<!--选项卡逻辑自己实现即可，此处未做处理-->
		<view :class="{'tui-order-list':scrollTop>=0}">
			<view class="tui-order-item" v-for="(item, index) in orderList" :key="index">
				<tui-list-cell :hover="false" :lineLeft="false">
					<view class="tui-goods-title">
						<view>下单时间：{{item.created_at}}</view>
						<view class="tui-order-status">[{{orderType(item.type)}}]</view>
						<view class="tui-order-status">{{orderStatus(item.status)}}</view>
					</view>
				</tui-list-cell>
				<!-- <block v-for="(item,index) in 2" :key="index"> -->
				<block>
					<tui-list-cell padding="0" @click="detail(item.id)">
						<view class="tui-goods-item">
							<image :src="imgURL(item)" class="tui-goods-img"></image>
							<view class="tui-goods-center">
								<view class="tui-goods-name">{{comTitle(item)}}</view>
								<!-- <view class="tui-goods-attr">黑色，50ml</view> -->
							</view>
							<view class="tui-price-right">
								<view v-if="item.type === 7">阳光币{{item.points}}</view>
								<view v-else>￥{{item.price / 100}}</view>
								<view>x{{item.size}}</view>
							</view>
						</view>
					</tui-list-cell>
				</block>
				<tui-list-cell :hover="false" :last="true">
					<view class="tui-goods-price" v-if="item.type === 7">
						<view class="tui-size-24">合计：</view>
						<view class="tui-price-large">{{countPrice(item)}}</view>
					</view>
					<view class="tui-goods-price" v-else>
						<view v-if="item.play_way === 1">配送费{{item.express_price / 100}}元, 合计：</view>
						<view v-if="item.play_way === 2">配送费{{item.express_price / 100}}元, 余额支付{{item.balance_price / 100}}元, 合计：</view>
						<view class="tui-size-24">￥</view>
						<view class="tui-price-large">{{countPrice(item)}}</view>
						<!-- <view class="tui-size-24">.00</view> -->
					</view>
				</tui-list-cell>
				<view class="tui-order-btn">
					<!-- 待付款订单 -->
					<view class="tui-btn-ml" v-if="item.status === 1">
						<tui-button @tap="OnCancel(item.id)" type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">取消订单</tui-button>
					</view>
					<view class="tui-btn-ml" v-if="item.status === 1">
						<tui-button @tap="onPayOrder(item.id)" type="danger" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">立即支付</tui-button>
					</view>
					<!-- 待发货订单 -->
					<view class="tui-btn-ml" v-if="item.status === 2">
						<tui-button @tap="refundOrderClick(item.id)" type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">申请退款</tui-button>
					</view>
					<!-- 待收货订单 -->
					<view class="tui-btn-ml" v-if="item.status === 3">
						<tui-button @tap="detail" type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">查看物流</tui-button>
					</view>
					<view class="tui-btn-ml" v-if="item.status === 3">
						<tui-button @tap="onSubmitOrder(item.id)" type="danger" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">确认收货</tui-button>
					</view>
					<!-- 待评价订单 -->
					<view class="tui-btn-ml" v-if="item.status === 4">
						<tui-button @tap="refundOrderClick(item.id)" type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">申请退款</tui-button>
					</view>
					<view class="tui-btn-ml" v-if="item.status === 4">
						<tui-button @tap="clicksubmitComment(item.id)" type="danger" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">评价</tui-button>
					</view>
					<!-- 退款状态 -->
					<view class="tui-btn-ml" v-if="item.status === 6">
						<tui-button type="black" :plain="true" width="168rpx" height="56rpx" :size="26" shape="circle">{{refundText(item.refund_status)}}</tui-button>
					</view>
				</view>
			</view>

			<!-- <view class="tui-order-item">
				<tui-list-cell :hover="false" :lineLeft="false">
					<view class="tui-goods-title">
						<view>订单号：T201910000</view>
						<view class="tui-order-status">已取消</view>
					</view>
				</tui-list-cell>
				<block v-for="(item,index) in 1" :key="index">
					<tui-list-cell padding="0" @click="detail">
						<view class="tui-goods-item">
							<image :src="`/static/images/mall/product/${index+3}.jpg`" class="tui-goods-img"></image>
							<view class="tui-goods-center">
								<view class="tui-goods-name">欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜 30ml（欧莱雅彩妆 BB霜 粉BB 遮瑕疵 隔离）</view>
								<view class="tui-goods-attr">黑色，50ml</view>
							</view>
							<view class="tui-price-right">
								<view>￥298.00</view>
								<view>x2</view>
							</view>
						</view>
					</tui-list-cell>
				</block>
				<tui-list-cell :hover="false" :last="true">
					<view class="tui-goods-price">
						<view>共4件商品 合计：</view>
						<view class="tui-size-24">￥</view>
						<view class="tui-price-large">596</view>
						<view class="tui-size-24">.00</view>
					</view>
				</tui-list-cell>
				<view class="tui-order-btn">
					<view class="tui-btn-ml">
						<tui-button type="black" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">删除订单</tui-button>
					</view>
					<view class="tui-btn-ml">
						<tui-button type="danger" :plain="true" width="148rpx" height="56rpx" :size="26" shape="circle">再次购买</tui-button>
					</view>
				</view>
			</view> -->

		</view>
		<!--加载loadding-->
		<tui-loadmore :visible="loadding" :index="3" type="red"></tui-loadmore>
		<tui-nomore :visible="!pullUpOn" bgcolor="#fafafa"></tui-nomore>
		<!--加载loadding-->

		<tui-modal :show="cancelOrder" @click="cancelOrderOK" @cancel="cancelOrderOff" content="订单取消后不可恢复，确定取消吗？" color="#333" :size="32"></tui-modal>

		<tui-modal :show="submitOrder" @click="submitOrderOK" @cancel="submitOrderOff" content="您要确认收货这笔订单吗？" color="#333" :size="32"></tui-modal>

		<tui-modal :show="refundOrder" @cancel="refundOrderOff" :custom="true" :fadein="true">
			<view class="tui-modal-custom">
				<view class="tui-prompt-title">申请订单退款</view>
				<input v-if="refundOrder" placeholder="请输入退款理由" class="tui-input" v-model="refund_text" />
				<tui-buttons shape="circle" @click="refundOrderOK" size="small">确认退款</tui-buttons>
			</view>
		</tui-modal>

		<tui-modal :show="submitComment" @cancel="submitCommentOff" :custom="true" :fadein="true">
			<view class="tui-modal-custom">
				<view class="tui-prompt-title">商品评价</view>
				<input v-if="submitComment" placeholder="请输入评价内容" class="tui-input" v-model="content_text" />
				<tui-buttons shape="circle" @click="submitCommentOK" size="small">提交评价</tui-buttons>
			</view>
		</tui-modal>


	</view>
</template>

<script>
	import tuiTabs from "@/components/tui-tabs/tui-tabs"
	import tuiButton from "@/components/extend/button/button"
	import tuiLoadmore from "@/components/loadmore/loadmore"
	import tuiNomore from "@/components/nomore/nomore"
	import tuiListCell from "@/components/list-cell/list-cell"
	import tuiSticky from "@/components/sticky/sticky"
	import tuiModal from "@/components/modal/modal"
	import tuiButtons from "@/components/button/button"
	export default {
		components: {
			tuiTabs,
			tuiButton,
			tuiLoadmore,
			tuiNomore,
			tuiListCell,
			tuiSticky,
			tuiModal,
			tuiButtons
		},
		data() {
			return {
				tabs: [{
					name: "全部"
				}, {
					name: "待付款"
				}, {
					name: "待发货"
				}, {
					name: "待收货"
				}, {
					name: "待评价"
				}],
				currentTab: 0,
				pageIndex: 1,
				loadding: false,
				pullUpOn: true,
				scrollTop: 0,
				page: 1,
				totalPage: null,
				orderList: [],
				cancelOrder: false,
				cancel_id: 0,
				refundOrder: false,
				refund_text: '',
				refund_id: 0,
				submitOrder: false,
				order_id: 0,
				submitComment: false,
				content_text: '',
				comment_id: 0
			}
		},
		onLoad(options) {
			console.log('->options', options)
			this.currentTab = Number(options.index)
			if(this.currentTab === 0) {
				this.getUserOrderList(this.page);
			} else {
				this.getUserOrderList(this.page, this.currentTab);
			}
		},
		computed: {
			orderStatus: function() {
				return function(item) {
					switch(item) {
						case 1: { return '待付款' }
						case 2: { return '待发货' }
						case 3: { return '待收货' }
						case 4: { return '待评价' }
						case 5: { return '已完成' }
						case 6: { return '退款/售后' }
					}
				}
			},
			orderType: function() {
				return function(item) {
					switch(item) {
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
				}
			},
			countPrice: function() {
				return function(item) {
					if(item.type === 7) return item.hx_exercise_points.points
					if(item.price === 0) return '0.00'
					return ((item.price * item.size) + item.express_price - item.balance_price) / 100
				}
			},
			refundText: function() {
				return function(item) {
					switch(item) {
						case 1: { return '退款/审核中' }
						case 2: { return '退款成功' }
						case 3: { return '审核失败' }
					}
				}
			},
			imgURL: function() {
				return function(item) {
					// JSON.parse(item.hx_goods.img_url)[0]
					// item.hx_user_member.img_url
					if(item.type === 3) return JSON.parse(item.hx_exercise_bargain.hx_goods.img_url)[0]
					if(item.type === 4) return JSON.parse(item.hx_exercise_fight.hx_goods.img_url)[0]
					if(item.type === 6) return JSON.parse(item.hx_exercise_time_limit.hx_goods.img_url)[0]
					if(item.type === 7) return JSON.parse(item.hx_exercise_points.hx_goods.img_url)[0]
					if(item.type === 8) return item.hx_merchant.logo_url
					if(item.type === 9) return item.hx_merchant.logo_url
					if(item.hx_goods !== null) return JSON.parse(item.hx_goods.img_url)[0]
					if(item.hx_user_member !== null) return item.hx_user_member.img_url
				}
			},
			comTitle: function() {
				return function(item) {
					// item.hx_goods.title
					if(item.type === 3) return item.hx_exercise_bargain.hx_goods.title
					if(item.type === 4) return item.hx_exercise_fight.hx_goods.title
					if(item.type === 6) return item.hx_exercise_time_limit.hx_goods.title
					if(item.type === 7) return item.hx_exercise_points.hx_goods.title
					if(item.type === 8) return item.hx_merchant.title
					if(item.type === 9) return item.hx_merchant.title
					if(item.hx_goods !== null) return item.hx_goods.title
					if(item.hx_user_member !== null) return item.hx_user_member.name
				}
			}
		},
		methods: {
			change(e) {
				this.currentTab = e.index
				this.page = 1;
				this.getUserOrderList(this.page, this.currentTab);
			},
			detail(id) {
				uni.navigateTo({
					url: `/pages/Ahuixiang/orderDetail/orderDetail?id=${id}`
				})
			},
			getUserOrderList(page, index) {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);

						console.log('->index', index)
						//
						if(index !== undefined) {
							page = page + '&index=' + index
						}

						uni.request({
							url: `${getApp().globalData.URL}/clientOrder/getUserOrderList?page=${page}`,
							method: 'GET',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							success: (res) => {
								// SUCCESS
								if(this.page === 1) {
									this.orderList = res.data.data
									if(!res.data.data) {
										this.pullUpOn = false
									}
								} else {
									this.orderList = res.data.data ? this.orderList.concat(res.data.data) : [];
								}
								this.totalPage = res.data.totalPage;
								this.loadding = false
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
			},
			OnCancel(id) {
				this.cancelOrder = true
				this.cancel_id = id;
			},
			cancelOrderOK(e) {
				let index = e.index;
				if (index === 0) {
					// this.tui.toast("你点击了取消按钮")
				} else {
					// this.tui.toast("你点击了确定按钮")
					// 执行取消订单操作
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientOrder/cancelOrder`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									id: this.cancel_id
								},
								success: (res) => {
									// SUCCESS
									this.page = 1;
									this.getUserOrderList(this.page);
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
				this.cancelOrderOff();
			},
			cancelOrderOff() {
				this.cancelOrder = false;
			},
			onPayOrder(id) {
				let thls = this;
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientOrder/payOrder`,
							method: 'POST',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							data: {
								id
							},
							success: (res) => {
								if(res.data.message === 'SUCCESS') {
									let pay = res.data.pay;
									// 唤醒支付
									uni.getProvider({
										service: 'payment',
										success: function (res) {
											uni.requestPayment({
												provider: res.provider[0],
												timeStamp: pay.timeStamp,
												nonceStr: pay.nonceStr,
												package: pay.package,
												signType: pay.signType,
												paySign: pay.paySign,
												success: function(res) {
													console.log('->succ', res)
													thls.page = 1;
													thls.getUserOrderList(thls.page);
													uni.redirectTo({
														url: "/pages/Ahuixiang/success/success"
													})
												},
												fail: (fail) => {
													console.log('->失败', fail)
													uni.showToast({
														title: '付款失败！',
														icon: 'none',
														duration: 2000
													});
													thls.page = 1;
													thls.getUserOrderList(thls.page);
												}
											})
										}
									});
								}
								
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
			},
			refundOrderClick(id) {
				this.refund_id = id;
				this.refundOrder = true;
			},
			refundOrderOff() {
				this.refundOrder = false;
			},
			refundOrderOK() {
				if(this.refund_text !== '') {
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientOrder/refundOrder`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									id: this.refund_id,
									refund_text: this.refund_text
								},
								success: (res) => {
									if(res.data.message === 'SUCCESS') {
										uni.showToast({ title: '提交成功！', duration: 1000 });
										this.refundOrderOff()
										this.page = 1;
										this.getUserOrderList(this.page);
									}
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
				} else {
					uni.showToast({
						title: '退款理由不能为空！',
						icon: 'none',
						duration: 2000
					});
				}
			},
			submitOrderOff() {
				this.submitOrder = false;
			},
			onSubmitOrder(id) {
				this.order_id = id;
				this.submitOrder = true;
			},
			submitOrderOK(e) {
				let index = e.index;
				if (index === 0) {
					// this.tui.toast("你点击了取消按钮")
				} else {
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientOrder/onSubmitOrder`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									id: this.order_id,
								},
								success: (res) => {
									if(res.data.message === 'SUCCESS') {
										uni.showToast({ title: '确认收货成功！', duration: 1000 });
										this.page = 1;
										this.getUserOrderList(this.page);
									}
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
				this.submitOrderOff()
			},
			submitCommentOff() {
				this.submitComment = false;
			},
			clicksubmitComment(id) {
				this.comment_id = id;
				this.submitComment = true;
			},
			submitCommentOK() {
				if(this.content_text !== '') {
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientOrder/submitOrderComment`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									id: this.comment_id,
									content_text: this.content_text
								},
								success: (res) => {
									if(res.data.message === 'SUCCESS') {
										uni.showToast({ title: '评价成功！', duration: 1000 });
										this.submitCommentOff()
										this.page = 1;
										this.getUserOrderList(this.page);
									}
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
				} else {
					uni.showToast({
						title: '商品评价内容不能为空！',
						icon: 'none',
						duration: 2000
					});
				}
			}
		},
		 //页面相关事件处理函数--监听用户下拉动作
		onPullDownRefresh: function() {
			// this.newsList = this.dataSources;
			// this.currentTab = 0;
			this.page = 1;
			this.pullUpOn = true;
			this.loadding = true;
			this.getUserOrderList(this.page, this.currentTab);
			uni.stopPullDownRefresh();
			let options = {
				msg: "刷新成功!",
				duration: 2000,
				type: "translucent"
			};
			setTimeout(() => {
				// this.$refs.toast.showTips(options);
			}, 300);
		},
		// 页面上拉触底事件的处理函数
		onReachBottom: function() {
			if (!this.pullUpOn) return;
			this.loadding = true;
			if (this.page == this.totalPage) {
				this.loadding = false;
				this.pullUpOn = false
			} else {
				this.page = this.page + 1;
				this.getUserOrderList(this.page, this.currentTab);
			}
		},
		onPageScroll(e) {
			this.scrollTop = e.scrollTop;
		}
	}
</script>

<style>
	.container {
		padding-bottom: env(safe-area-inset-bottom);
	}

	.tui-order-list {
		margin-top: 80rpx;
	}

	.tui-order-item {
		margin-top: 20rpx;
		border-radius: 10rpx;
		overflow: hidden;
	}

	.tui-goods-title {
		width: 100%;
		font-size: 28rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.tui-order-status {
		color: #888;
		font-size: 26rpx;
	}

	.tui-goods-item {
		width: 100%;
		padding: 20rpx 30rpx;
		box-sizing: border-box;
		display: flex;
		justify-content: space-between;
	}

	.tui-goods-img {
		width: 120rpx;
		height: 120rpx;
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
		font-weight: 500;
	}

	.tui-order-btn {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		background: #fff;
		padding: 10rpx 30rpx 20rpx;
		box-sizing: border-box;
	}

	.tui-btn-ml {
		margin-left: 20rpx;
	}

	
	.tui-modal-custom {
		text-align: center
	}

	.tui-tips-img {
		width: 100rpx;
		height: 100rpx;
		margin-top: 20rpx;
	}

	.tui-modal-custom-text {
		font-size: 30rpx;
		color: #333;
		padding: 30rpx 0
	}

	.tui-prompt-title {
		padding-bottom: 20rpx;
		font-size: 34rpx;
	}
	.tui-input {
		margin: 30rpx 40rpx;
		border-bottom: 1rpx solid #E6E6E6;
		padding-bottom: 20rpx;
		font-size: 32rpx;
	}
</style>
