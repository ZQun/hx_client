<template>
	<view class="container">
		<!-- #ifdef MP || H5-->
		<view class="tui-edit-goods">
			<view>购物车共<text class="tui-goods-num">{{goodsList.length}}</text>件商品</view>
			<view>
				<tui-button type="gray" :plain="true" shape="circle" width="160rpx" height="60rpx" :size="24" @click="editGoods">{{isEdit?"完成":"编辑商品"}}</tui-button>
			</view>
		</view>
		<!-- #endif -->
		<checkbox-group @change="allSelect">
			<view class="tui-cart-cell  tui-mtop" v-for="(item,index) in goodsList" :key="index">
				<!-- <view class="tui-activity" v-if="index%2==0">
					<view class="tui-bold">满3件享受优惠</view>
					<view class="tui-buy">去凑单<tui-icon name="arrowright" :size="18" color="#333"></tui-icon>
					</view>
				</view> -->
				<tui-swipe-action :actions="actions" @click="handlerButton" :params="item">
					<template v-slot:content>
						<view class="tui-goods-item">
							<label class="tui-checkbox">
								<checkbox :value="index" :checked="item.checked" color="#fff"></checkbox>
							</label>
							<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="tui-goods-img" />
							<view class="tui-goods-info">
								<view class="tui-goods-title">
									{{item.hx_goods.title}}
								</view>
								<!-- <view class="tui-goods-model">
									<view class="tui-model-text">{{index%2==0?"440ml;10件;套装":"500ml;2支"}}</view>
									<tui-icon name="arrowdown" :size="16" color="#333"></tui-icon>
								</view> -->
								<view class="tui-price-box">
									<view class="tui-goods-price">￥{{price(item)}}</view>
									<view class="tui-scale">
										<tui-numberbox :value="item.num" :height="40" :width="74" :min="1" :index="index" @change="changeNum"></tui-numberbox>
									</view>
								</view>
							</view>
						</view>
					</template>
				</tui-swipe-action>
				<!-- <view class="tui-sub-info" v-if="index%2==0">赠品：柔色尽情丝柔口红唇膏1支柔色尽情丝柔口红唇膏1支</view> -->
			</view>
		</checkbox-group>

		<!--商品失效-->
		<!-- <view class="tui-cart-cell  tui-mtop">
			<view class="tui-activity">
				<view class="tui-bold">失效商品</view>
				<view class="tui-buy">
					<tui-button type="gray" :plain="true" shape="circle" width="200rpx" height="56rpx" :size="24">清空失效商品</tui-button>
				</view>
			</view>
			<view :class="{'tui-invalid-ptop':index!==0}" v-for="(item,index) in 2" :key="index">
				<tui-swipe-action :actions="actions2">
					<template v-slot:content>
						<view class="tui-goods-item">
							<view class="tui-checkbox tui-invalid-pr">
								<view class="tui-invalid-text">失效</view>
							</view>
							<image src="/static/images/mall/product/4.jpg" class="tui-goods-img opcity" />
							<view class="tui-goods-info">
								<view class="tui-goods-title tui-gray">
									欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜彩粉嫩透亮修颜霜透亮修颜霜透亮修颜霜
								</view>
								<view class="tui-price-box tui-flex-center">
									<view class="tui-goods-invalid">产品失效</view>
									<view class="tui-btn-alike">
										<tui-button type="white" :plain="true" shape="circle" width="120rpx" height="48rpx" :size="24">找相似</tui-button>
									</view>
								</view>
							</view>
						</view>
					</template>
				</tui-swipe-action>
			</view>
		</view> -->

		<!--猜你喜欢-->

		<!-- <tui-divider :size="28" :bold="true" color="#333" width="50%">
			<tui-icon name="like" :size="18" color="#e41f19"></tui-icon>
			<text class="tui-youlike">猜你喜欢</text>
		</tui-divider>
		<view class="tui-product-list">
			<view class="tui-product-container">
				<block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2!=0">
					<view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail">
						<image :src="'/static/images/mall/product/'+item.img+'.jpg'" class="tui-pro-img" mode="widthFix" />
						<view class="tui-pro-content">
							<view class="tui-pro-tit">{{item.name}}</view>
							<view>
								<view class="tui-pro-price">
									<text class="tui-sale-price">￥{{item.sale}}</text>
									<text class="tui-factory-price">￥{{item.factory}}</text>
								</view>
								<view class="tui-pro-pay">{{item.payNum}}人付款</view>
							</view>
						</view>
					</view>
				</block>
			</view>
			<view class="tui-product-container">
				<block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2==0">
					<view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail">
						<image :src="'/static/images/mall/product/'+item.img+'.jpg'" class="tui-pro-img" mode="widthFix" />
						<view class="tui-pro-content">
							<view class="tui-pro-tit">{{item.name}}</view>
							<view>
								<view class="tui-pro-price">
									<text class="tui-sale-price">￥{{item.sale}}</text>
									<text class="tui-factory-price">￥{{item.factory}}</text>
								</view>
								<view class="tui-pro-pay">{{item.payNum}}人付款</view>
							</view>
						</view>
					</view>
				</block>
			</view>
		</view> -->

		<!--tabbar-->
		<view class="tui-tabbar">
			<view class="tui-checkAll">
				<!-- <checkbox-group @change="allSelect">
					<label class="tui-checkbox">
						<checkbox value="全选" color="#fff"></checkbox>
						<text class="tui-checkbox-pl">全选</text>
					</label>
				</checkbox-group> -->
				<view class="tui-total-price" v-if="!isEdit">合计:<text class="tui-bold">￥{{countPrice}}</text> </view>
			</view>
			<view>
				<tui-button width="200rpx" height="70rpx" :size="30" type="green" shape="circle" v-if="!isEdit" @click="btnPay">去结算({{checkedArr.length}})</tui-button>
				<tui-button @tap="deleteCart" width="200rpx" height="70rpx" :size="30" type="danger" shape="circle" :plain="true" v-else>删除</tui-button>
			</view>
		</view>
		<!--加载loadding-->
		<tui-loadmore :visible="loadding" :index="3" type="red"></tui-loadmore>
	</view>
</template>

<script>
	import tuiSwipeAction from "@/components/swipe-action/swipe-action"
	import tuiButton from "@/components/extend/button/button"
	import tuiNumberbox from "@/components/numberbox/numberbox"
	import tuiIcon from "@/components/icon/icon"
	import tuiDivider from "@/components/divider/divider"
	import tuiLoadmore from "@/components/loadmore/loadmore"
	export default {
		components: {
			tuiSwipeAction,
			tuiButton,
			tuiNumberbox,
			tuiIcon,
			tuiDivider,
			tuiLoadmore
		},
		data() {
			return {
				dataList: [{
					id: 1,
					buyNum:2
				}, {
					id: 2,
					buyNum:1
				}],
				goodsList: [],// 商品列表
				checkedArr: [],
				deleteArr: [],
				actions: [{
						name: '收藏',
						width: 64,
						color: '#fff',
						fontsize: 28,
						background: '#FFC600'
					},
					{
						name: '看相似',
						color: '#fff',
						fontsize: 28,
						width: 64,
						background: '#FF7035'
					},
					{
						name: '删除',
						color: '#fff',
						fontsize: 28,
						width: 64,
						background: '#F82400'
					}
				],
				actions2: [{
						name: '看相似',
						color: '#fff',
						fontsize: 28,
						width: 64,
						background: '#FF7035'
					},
					{
						name: '删除',
						color: '#fff',
						fontsize: 28,
						width: 64,
						background: '#F82400'
					}
				],
				isEdit: false,
				productList: [{
						img: 1,
						name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜 30ml（欧莱雅彩妆 BB霜 粉BB 遮瑕疵 隔离）",
						sale: 599,
						factory: 899,
						payNum: 2342
					},
					{
						img: 2,
						name: "德国DMK进口牛奶  欧德堡（Oldenburger）超高温处理全脂纯牛奶1L*12盒",
						sale: 29,
						factory: 69,
						payNum: 999
					},
					{
						img: 3,
						name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
						sale: 299,
						factory: 699,
						payNum: 666
					},
					{
						img: 4,
						name: "百雀羚套装女补水保湿护肤品",
						sale: 1599,
						factory: 2899,
						payNum: 236
					},
					{
						img: 5,
						name: "百草味 肉干肉脯 休闲零食 靖江精制猪肉脯200g/袋",
						sale: 599,
						factory: 899,
						payNum: 2399
					},
					{
						img: 6,
						name: "短袖睡衣女夏季薄款休闲家居服短裤套装女可爱韩版清新学生两件套 短袖粉色长颈鹿 M码75-95斤",
						sale: 599,
						factory: 899,
						payNum: 2399
					},
					{
						img: 1,
						name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜",
						sale: 599,
						factory: 899,
						payNum: 2342
					},
					{
						img: 2,
						name: "德国DMK进口牛奶",
						sale: 29,
						factory: 69,
						payNum: 999
					},
					{
						img: 3,
						name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
						sale: 299,
						factory: 699,
						payNum: 666
					},
					{
						img: 4,
						name: "百雀羚套装女补水保湿护肤品",
						sale: 1599,
						factory: 2899,
						payNum: 236
					}
				],
				pageIndex: 1,
				loadding: false,
				pullUpOn: true,
				userInfo: {}
			}
		},
		onLoad(options) {
			console.log('->')
			this.getShopCartList();
		},
		computed: {
			price: function(item) {
				return function(item) {
					if(this.userInfo.grade === 2 && item.hx_goods.vip_price) {
						return item.hx_goods.vip_price / 100;
					} else {
						return item.hx_goods.price / 100;
					}
				}
			},
			countPrice: function () {
				if(this.checkedArr.length !== 0) {
					let price = 0;
					this.checkedArr.forEach(item => {
						let index = Number(item);
						let grade = 'price';
						if (this.userInfo.grade === 2 && this.goodsList[index].hx_goods.vip_price) {
							grade = 'vip_price'
						}
						price = price + (this.goodsList[index].hx_goods[grade] * this.goodsList[index].num)
					})
					return price / 100
				} else {
					return 0
				}
			}
		},
		methods: {
			changeNum: function(e) {
				this.goodsList[e.index].num=e.value
				// 修改数据库数量
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientUser/updateShoppingNum?id=${this.goodsList[e.index].id}`,
							method: 'POST',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							data: {
								num: e.value
							},
							success: (res) => {
								// SUCCESS
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
			handlerButton: function(e) {
				let index = e.index;
				let item = e.item;
				this.tui.toast(`商品id：${item.id}，按钮index：${index}`);
			},
			editGoods: function() {
				// #ifdef H5 || MP
				this.isEdit = !this.isEdit;
				// #endif
			},
			detail: function() {
				uni.navigateTo({
					url: '/pages/Ahuixiang/productDetail/productDetail'
				})
			},
			btnPay(){
				if(this.checkedArr.length !== 0) {
					let value = this.checkedArr;
					let arr = [];
					value.forEach(item => {
						item = this.goodsList[Number(item)].id;
						arr.push(item);
					})
					// uni.$emit('cart', arr)
					uni.navigateTo({
						url: `/pages/Ahuixiang/submitOrder/submitOrder?cart=cart&arr=${JSON.stringify(arr)}`
					})
				} else {
					uni.showToast({
                        title: '请选择商品！',
                        icon: 'none',
                        duration: 2000
                    });
				}
			},
			allSelect(e) {
				if(!this.isEdit) {
					this.checkedArr = e.target.value;
				} else {
					let value = e.target.value;
					let arr = [];
					value.forEach(item => {
						item = this.goodsList[Number(item)].id;
						arr.push(item);
					})
					this.deleteArr = arr;
				}
			},
			deleteCart() {
				if(this.deleteArr.length !== 0) {
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientUser/deleteShopping`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: this.deleteArr,
								success: (res) => {
									if(res.data.message == 'SUCCESS') {
										uni.showToast({ title: '删除成功！', duration: 1000 });
										this.getShopCartList();
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
                        title: '请选择商品！',
                        icon: 'none',
                        duration: 2000
                    });
				}
			},
			getShopCartList() {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientUser/getShoppingList`,
							method: 'GET',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							// data: this.addForm,
							success: (res) => {
								// this.addressList = res.data !== '暂无数据' ? res.data : [];
								this.goodsList = res.data.get !== '暂无数据' ? res.data.get : [];
								this.goodsList.forEach(item => {
									item.checked = false
								})
								this.userInfo = res.data.user;
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
		},
		// onPullDownRefresh() {
		// 	setTimeout(() => {
		// 		uni.stopPullDownRefresh()
		// 	}, 200)
		// },
		onPullDownRefresh: function() {
			this.getShopCartList();
			console.log('-> load')
			let loadData = JSON.parse(JSON.stringify(this.productList));
			loadData = loadData.splice(0, 10)
			this.productList = loadData;
			this.pageIndex = 1;
			this.pullUpOn = true;
			this.loadding = false
			uni.stopPullDownRefresh()
		},
		onReachBottom: function() {
			if (!this.pullUpOn) return;
			this.loadding = true;
			if (this.pageIndex == 4) {
				this.loadding = false;
				this.pullUpOn = false
			} else {
				let loadData = JSON.parse(JSON.stringify(this.productList));
				loadData = loadData.splice(0, 10)
				if (this.pageIndex == 1) {
					loadData = loadData.reverse();
				}
				this.productList = this.productList.concat(loadData);
				this.pageIndex = this.pageIndex + 1;
				this.loadding = false
			}
		},
		onNavigationBarButtonTap(e) {
			this.isEdit = !this.isEdit;
			let text = this.isEdit ? "完成" : "编辑";
			// #ifdef APP-PLUS
			let webView = this.$mp.page.$getAppWebview();
			webView.setTitleNViewButtonStyle(0, {
				text: text
			});
			// #endif
		}
	}
</script>

<style>
	.container {
		padding-bottom: 120rpx;
	}

	.tui-mtop {
		margin-top: 24rpx;
	}

	.tui-edit-goods {
		width: 100%;
		border-radius: 12rpx;
		overflow: hidden;
		padding: 24rpx 30rpx 0 30rpx;
		box-sizing: border-box;
		display: flex;
		justify-content: space-between;
		align-items: center;
		color: #333;
		font-size: 24rpx;
	}

	.tui-goods-num {
		font-weight: bold;
		color: #e41f19;
	}

	.tui-cart-cell {
		width: 100%;
		border-radius: 12rpx;
		background: #FFFFFF;
		padding: 40rpx 0;
		overflow: hidden;
	}

	.tui-goods-item {
		display: flex;
		padding: 0 30rpx;
		box-sizing: border-box;
	}

	.tui-checkbox {
		/* width: 40rpx; */
		padding-right: 30rpx;
		display: flex;
		align-items: center;
	}

	/* #ifdef APP-PLUS || MP */
	.tui-checkbox .wx-checkbox-input {
		width: 36rpx;
		height: 36rpx;
		border-radius: 50%;
		margin: 0;
	}

	.tui-checkbox .wx-checkbox-input.wx-checkbox-input-checked {
		background: #F82400;
		width: 40rpx;
		height: 40rpx;
		border: none;
	}

	/* #endif */
	/* #ifdef H5 */
	>>>.tui-checkbox .uni-checkbox-input {
		width: 36rpx;
		height: 36rpx;
		border-radius: 50% !important;
		margin: 0;
	}

	>>>.tui-checkbox .uni-checkbox-input.uni-checkbox-input-checked {
		background: #F82400;
		width: 40rpx;
		height: 40rpx;
		border: none;
	}

	/* #endif */
	.tui-goods-img {
		width: 120rpx;
		height: 120rpx !important;
		border-radius: 12rpx;
		flex-shrink: 0;
		display: block;
	}

	.tui-goods-info {
		width: 100%;
		padding-left: 20rpx;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-between;
		box-sizing: border-box;
		overflow: hidden;
	}

	.tui-goods-title {
		white-space: normal;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
		font-size: 24rpx;
		color: #333;
	}

	.tui-goods-model {
		max-width: 100%;
		color: #333;
		background: #F5F5F5;
		border-radius: 40rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 16rpx;
		box-sizing: border-box;
	}

	.tui-model-text {
		max-width: 100%;
		transform: scale(0.9);
		transform-origin: 0 center;
		font-size: 24rpx;
		line-height: 32rpx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.tui-price-box {
		width: 100%;
		display: flex;
		align-items: flex-end;
		justify-content: space-between;
	}

	.tui-goods-price {
		font-size: 34rpx;
		font-weight: 500;
		color: #e41f19;
	}

	.tui-scale {
		transform: scale(0.8);
		transform-origin: 100% 100%;
	}

	.tui-activity {
		font-size: 24rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 30rpx 20rpx 100rpx;
		box-sizing: border-box;
	}

	.tui-buy {
		display: flex;
		align-items: center
	}

	.tui-bold {
		font-weight: bold;
	}

	.tui-sub-info {
		max-width: 532rpx;
		font-size: 24rpx;
		line-height: 24rpx;
		padding: 20rpx 30rpx 10rpx 30rpx;
		box-sizing: border-box;
		color: #333;
		transform: scale(0.8);
		transform-origin: 100% center;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		margin-left: auto
	}

	.tui-invalid-text {
		width: 66rpx;
		margin-right: 4rpx;
		text-align: center;
		font-size: 24rpx;
		color: #fff;
		background: rgba(0, 0, 0, .3);
		transform: scale(0.8);
		transform-origin: center center;
		border-radius: 4rpx;
		flex-shrink: 0;
	}

	.tui-invalid-pr {
		padding-right: 0 !important;
	}

	.tui-gray {
		color: #B2B2B2 !important;
	}

	.tui-goods-invalid {
		color: #555;
		font-size: 24rpx;
	}

	.tui-flex-center {
		align-items: center !important;
	}

	.tui-invalid-ptop {
		padding-top: 40rpx;
	}

	.tui-tabbar {
		width: 100%;
		height: 100rpx;
		background: #fff;
		position: fixed;
		left: 0;
		bottom: 0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 30rpx;
		box-sizing: border-box;
		font-size: 24rpx;
		z-index: 99999;
	}

	.tui-tabbar::before {
		content: '';
		width: 100%;
		border-top: 1rpx solid #d9d9d9;
		position: absolute;
		top: 0;
		left: 0;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
	}

	.tui-checkAll {
		display: flex;
		align-items: center;
	}

	.tui-checkbox-pl {
		padding-left: 12rpx;
	}

	.tui-total-price {
		font-size: 30rpx !important;
	}

	/*猜你喜欢*/
	.tui-youlike {
		padding-left: 12rpx
	}

	.tui-product-list {
		display: flex;
		justify-content: space-between;
		flex-direction: row;
		flex-wrap: wrap;
		box-sizing: border-box;
	}

	.tui-product-container {
		flex: 1;
		margin-right: 2%;
	}

	.tui-product-container:last-child {
		margin-right: 0;
	}

	.tui-pro-item {
		width: 100%;
		margin-bottom: 4%;
		background: #fff;
		box-sizing: border-box;
		border-radius: 12rpx;
		overflow: hidden;
	}

	.tui-pro-img {
		width: 100%;
		display: block;
	}

	.tui-pro-content {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 20rpx;
	}

	.tui-pro-tit {
		color: #2e2e2e;
		font-size: 26rpx;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.tui-pro-price {
		padding-top: 18rpx;
	}

	.tui-sale-price {
		font-size: 34rpx;
		font-weight: 500;
		color: #e41f19;
	}

	.tui-factory-price {
		font-size: 24rpx;
		color: #a0a0a0;
		text-decoration: line-through;
		padding-left: 12rpx;
	}

	.tui-pro-pay {
		padding-top: 10rpx;
		font-size: 24rpx;
		color: #656565;
	}
</style>
