<template>
	<view>
		<Menu :current="4"></Menu>

		<!--header-->
		<view class="tui-header-box" :style="{height:height+'px',background:'rgba(255,255,255,'+opcity+')'}">
			<view class="tui-header" :style="{paddingTop:top+'px', opacity:opcity}">
				我的
			</view>
			<!-- #ifndef MP -->
			<view class="tui-header-icon" :style="{marginTop:top+'px'}" @tap="href(1)">
				<view class="tui-icon-box tui-icon-message">
					<tui-icon name="message" :color="opcity>0.02?`rgba(85,85,85,${opcity})`:'#fff'" :size="26"></tui-icon>
					<view class="tui-badge" :class="[opcity>0.05?'tui-badge-red':'tui-badge-white']">1</view>
				</view>
				<view class="tui-icon-box tui-icon-setup" @tap="href(2)">
					<tui-icon name="setup" :color="opcity>0.02?`rgba(85,85,85,${opcity})`:'#fff'" :size="26"></tui-icon>
				</view>
			</view>
			<!-- #endif -->
		</view>
		<!--header-->
		<view class="tui-mybg-box">
			<image src="/static/images/mall/my/img_bgs_3x.png" class="tui-my-bg" mode="widthFix"></image>
			<view class="tui-header-center">
				<!-- <image v-if="login" :src="avatarUrl" class="tui-avatar" @tap="href(3)"></image> -->
				<image v-if="login" :src="avatarUrl" class="tui-avatar"></image>
				<image v-if="!login" src="/static/images/my/mine_def_touxiang_3x.png" class="tui-avatar" @tap="loginUser"></image>
				<view class="tui-info">
					<view class="tui-nickname">
						<view v-if="login">{{nickName}}</view>
						<view v-if="!login" @tap="loginUser">暂未登录</view>
						<image @tap="memberPath" v-if="login && userInfo.grade !== 1" src="/static/images/mall/my/icon_vip_3x.png" class="tui-img-vip"></image>
						<image @tap="memberPath" v-if="!login || userInfo.grade === 1" src="/static/images/mall/my/icon_vip_default_3x.png" class="tui-img-vip"></image>
					</view>
					<view v-if="login" class="tui-explain">信用分：{{userInfo.credit}}</view>
					<view v-if="login && userInfo.grade !== 1" class="tui-explain">到期时间：{{userInfo.end_time}}</view>
					<view v-if="!login" @tap="loginUser" class="tui-explain">这家伙很懒…</view>
				</view>
				<!-- #ifndef MP -->
				<view class="tui-btn-edit">
					<tui-button type="white" :plain="true" shape="circle" width="92rpx" height="40rpx" :size="22" @click="href(3)">编辑</tui-button>
				</view>
				<!-- #endif -->
				<!-- #ifdef MP -->
				<view class="tui-set-box">
					<!-- <view class="tui-icon-box tui-icon-message" @tap="href(1)">
						<tui-icon name="message" color="#fff" :size="26"></tui-icon>
						<view class="tui-badge tui-badge-white">1</view>
					</view> -->
					<!-- <view class="tui-icon-box tui-icon-setup" @tap="href(2)">
						<tui-icon name="setup" color="#fff" :size="26"></tui-icon>
					</view> -->
				</view>
				<!-- #endif -->
			</view>
			<view class="tui-header-btm">
				<view class="tui-btm-item">
					<view class="tui-btm-num">{{userInfo.integral}}</view>
					<view class="tui-btm-text">阳光币</view>
				</view>
				<view class="tui-btm-item">
					<view class="tui-btm-num">{{userInfo.balance / 100}}</view>
					<view class="tui-btm-text">收益余额</view>
				</view>
				<view class="tui-btm-item">
					<!-- <view class="tui-btm-num">3</view>
					<view class="tui-btm-text">喜欢的内容</view> -->
				</view>
				<view style="border: solid 1px #ccc; border-radius: 15rpx; padding: 8rpx 15rpx;" class="tui-btm-item" @tap="qian">
					<view class="tui-btm-num">每日签到</view>
					<!-- <view class="tui-btm-text">足迹</view> -->
				</view>
			</view>
		</view>
		<view class="tui-content-box">
			<view class="tui-box tui-order-box">
				<tui-list-cell :arrow="true" padding="0" :lineLeft="false" @click="href(4, 0)">
					<view class="tui-cell-header">
						<view class="tui-cell-title">我的订单</view>
						<view class="tui-cell-sub">查看全部订单</view>
					</view>
				</tui-list-cell>
				<view class="tui-order-list">
					<view class="tui-order-item" @tap="href(4, 1)">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_daifukuan_3x.png" class="tui-order-icon"></image>
							<!-- <view class="tui-badge tui-badge-red">1</view> -->
						</view>
						<view class="tui-order-text">待付款</view>
					</view>
					<view class="tui-order-item" @tap="href(4, 2)">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_daifahuo_3x.png" class="tui-order-icon"></image>
							<!-- <view class="tui-badge tui-badge-red">1</view> -->
						</view>
						<view class="tui-order-text">待发货</view>
					</view>
					<view class="tui-order-item" @tap="href(4, 3)">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_daishouhuo_3x.png" class="tui-order-icon"></image>
						</view>
						<view class="tui-order-text">待收货</view>
					</view>
					<view class="tui-order-item" @tap="href(4, 4)">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_pingjia_3x.png" class="tui-order-icon"></image>
							<!-- <view class="tui-badge tui-badge-red" v-if="false">12</view> -->
						</view>
						<view class="tui-order-text">评价</view>
					</view>
					<view class="tui-order-item" @tap="href(4, 0)">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_tuikuan_3x.png" class="tui-order-icon"></image>
							<!-- <view class="tui-badge tui-badge-red">2</view> -->
						</view>
						<view class="tui-order-text">退款/售后</view>
					</view>
				</view>
			</view>

			<view class="tui-box tui-assets-box">
				<tui-list-cell padding="0" :last="true" :hover="false">
					<view class="tui-cell-header">
						<view class="tui-cell-title">功能入口</view>
					</view>
				</tui-list-cell>
				<view class="tui-order-list tui-assets-list">
					<!-- <view class="tui-order-item">
						<view class="tui-assets-num">
							<text>3</text>
							<view class="tui-badge-dot"></view>
						</view>
						<view class="tui-assets-text">优惠券</view>
					</view>
					<view class="tui-order-item">
						<view class="tui-assets-num">
							<text>7</text>
							<view class="tui-badge-dot"></view>
						</view>
						<view class="tui-assets-text">积分</view>
					</view>
					-->
					<view class="tui-order-item" @tap="goPathCart">
						<view class="tui-assets-num">
							<text>{{userInfo.cart}}</text>
						</view>
						<view class="tui-assets-text">购物车</view>
					</view>

					<view class="tui-order-item" @tap="myOrderPath(0)">
						<view class="tui-assets-num">
							<text>{{userInfo.bargain}}</text>
						</view>
						<view class="tui-assets-text">砍价</view>
					</view>
					<view class="tui-order-item" @tap="myOrderPath(1)">
						<view class="tui-assets-num">
							<text>{{userInfo.fight}}</text>
						</view>
						<view class="tui-assets-text">拼团(发起)</view>
					</view>
					<view class="tui-order-item" @tap="myOrderPath(2)">
						<view class="tui-assets-num">
							<text>{{userInfo.fight_user}}</text>
							<!-- <view class="tui-badge-dot"></view> -->
						</view>
						<view class="tui-assets-text">拼团(参团)</view>
					</view>
				</view>
			</view>

			<view class="tui-box tui-tool-box">
				<tui-list-cell :arrow="true" padding="0" :lineLeft="false">
					<view class="tui-cell-header">
						<view class="tui-cell-title">快捷导航</view>
						<!-- <view class="tui-cell-sub">查看更多</view> -->
					</view>
				</tui-list-cell>
				<view class="tui-order-list tui-flex-wrap">


					<view v-for="(item, index) in lists" :key="index" @tap="myMenuClick(item.tap)" class="tui-tool-item">
						<view class="tui-icon-box">
							<image :src="item.icon" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">{{item.name}}</view>
					</view>


					<!-- <view @tap="merchantPath" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_purse_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">商家入驻</view>
					</view>
					<view @tap="memberPath" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_ticket_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">加入会员</view>
					</view>
					<view @tap="userForum" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_kaipiao_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">帖子管理</view>
					</view>
					<view class=" tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_kefu_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">举报管理</view>
					</view>
					<view @tap="pathExercise(4)" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_gift_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">积分兑换</view>
					</view>
					<view @tap="pathExercise(3)" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_fanxian_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">限时秒杀</view>
					</view>
					<view @tap="pathExercise(2)" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_tuan_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">超值拼团</view>
					</view>
					<view @tap="pathExercise(1)" class="tui-tool-item">
						<view class="tui-icon-box">
							<image src="/static/images/mall/my/icon_zhihuan_3x.png" class="tui-tool-icon"></image>
						</view>
						<view class="tui-tool-text">砍价活动</view>
					</view> -->
				</view>
			</view>

			<!--为你推荐-->
			<!-- <tui-divider :size="28" :bold="true" color="#333" width="50%">为你推荐</tui-divider>
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
			</view>
			<tui-loadmore :visible="loadding" :index="3" type="red"></tui-loadmore> -->

		</view>
	</view>
</template>

<script>
import tuiIcon from "@/components/icon/icon"
import tuiButton from "@/components/extend/button/button"
import tuiListCell from "@/components/list-cell/list-cell"
import tuiDivider from "@/components/divider/divider"
import tuiLoadmore from "@/components/loadmore/loadmore"
import Menu from '../menu/menu.vue'
import { Base64 } from 'js-base64'

export default {
	components: {
		tuiIcon,
		tuiButton,
		tuiListCell,
		tuiDivider,
		tuiLoadmore,
		Menu,
	},
	onLoad: function (options) {
		// 存储到访者来源
		if (options.path) {
			uni.setStorageSync('path', options.path);
		}

		let obj = {};
		// #ifdef MP-WEIXIN
		obj = wx.getMenuButtonBoundingClientRect();
		// #endif
		// #ifdef MP-BAIDU
		obj = swan.getMenuButtonBoundingClientRect();
		// #endif
		// #ifdef MP-ALIPAY
		my.hideAddToDesktopMenu();
		// #endif

		uni.getSystemInfo({
			success: (res) => {
				this.width = obj.left || res.windowWidth;
				this.height = obj.top ? (obj.top + obj.height + 8) : (res.statusBarHeight + 44);
				this.top = obj.top ? (obj.top + (obj.height - 32) / 2) : (res.statusBarHeight + 6);
				this.scrollH = res.windowWidth * 0.6
			}
		})

		this.selectMylist()
	},
	onShareAppMessage(res) {
		// 分享带分享者id
		let title = this.searchKey
		let path = '/pages/Ahuixiang/my/my'

		try {
			const value = uni.getStorageSync('token');
			if (value) {
				let id = JSON.parse(value).id;
				return { title, path: `${path}?path=${Base64.encode(id)}` }
			} else {
				return { title, path: `${path}` }
			}
		} catch (e) {
			return { title, path: `${path}` }
		}
	},
	onShow() {
		// 获取用户信息
		this.getUserInfo();
	},
	data() {
		return {
			height: 64, //header高度
			top: 0, //标题图标距离顶部距离
			scrollH: 0, //滚动总高度
			opcity: 0,
			iconOpcity: 0.5,
			productList: [
				{
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
			avatarUrl: '',
			nickName: '',
			login: false,
			userInfo: {
				credit: 0,
				integral: 0,
				balance: 0,
				grade: 1
			},
			lists: []
		}
	},
	methods: {
		goPathCart() {
			uni.navigateTo({
				url: '/pages/Ahx-extend/shopcart/shopcart'
			})
		},
		myOrderPath(index) {
			uni.navigateTo({
				url: '/pages/Ahx-user/myOrder/myOrder?index=' + index
			})
		},
		href(page, index) {
			let url = "";
			switch (page) {
				case 1:
					break;
				case 2:
					url = "/pages/Ahuixiang/set/set"
					break;
				case 3:
					url = "/pages/Ahuixiang/userInfo/userInfo"
					break;
				case 4:
					url = "/pages/Ahuixiang/myOrder/myOrder"
					break;
				default:
					break;
			}
			if (url) {
				if (index !== undefined) {
					url = url + '?index=' + index
				}
				uni.navigateTo({
					url: url
				})
			} else {
				this.tui.toast("功能尚未完善~")
			}
		},
		pathExercise(index) {
			let path = {
				1: `/pages/Ahx-extend/productList/productList?searchKey=砍价专场&type=bargain`,
				2: `/pages/Ahx-extend/productList/productList?searchKey=拼团专场&type=fight`,
				3: `/pages/Ahx-extend/productList/productList?searchKey=秒杀专场&type=time`,
				4: `/pages/Ahx-extend/productList/productList?searchKey=阳光币专场&type=points`,
			}
			uni.navigateTo({ url: path[index] })
		},
		detail: function () {
			uni.navigateTo({
				url: '/pages/Ahuixiang/productDetail/productDetail'
			})
		},
		memberPath() {
			uni.navigateTo({
				url: '/pages/Ahx-extend/productDetail/memberDetail'
			})
		},
		userForum() {
			uni.navigateTo({
				url: '/pages/Ahx-extend/forum/forum.user'
			})
		},
		qian() {
			uni.getStorage({
				key: 'token',
				success: (res) => {
					this.token = JSON.parse(res.data);

					uni.navigateTo({
						url: '/pages/Ahx-user/user/qian'
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
		myMenuClick(tap) {
			if(tap === 'huodong') {
				this.merchantPath()
				return
			}
			uni.navigateTo({ url: tap })
		},
		merchantPath() {
			uni.getStorage({
				key: 'token',
				success: (res) => {
					this.token = JSON.parse(res.data);

					try {
						const value = uni.getStorageSync('path');
						if (value) {
							// 绑定path
							uni.request({
								url: `${getApp().globalData.URL}/clientMerchant/path`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: {
									path: value
								},
								success: (res) => { }
							})
						}
					} catch (e) { }

					uni.redirectTo({
						url: `/pages/Ahx-extend/huodong/huodong?token=${this.token.token}`
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
		getUserInfo(login) {
			uni.getStorage({
				key: 'token',
				success: (res) => {
					this.token = JSON.parse(res.data);

					uni.request({
						url: `${getApp().globalData.URL}/clientUser/getUserInfo`,
						method: 'GET',
						header: {
							'Authorization': "Bearer " + this.token.token
						},
						// data: this.addForm,
						success: (res) => {
							this.userInfo = res.data;
							// this.text = res.data.province +'-'+ res.data.city +'-'+ res.data.county;
						}
					})

					try {
						const avatarUrl = uni.getStorageSync('avatarUrl');
						const nickName = uni.getStorageSync('nickName');
						if (avatarUrl && nickName) {
							this.login = true;
							this.avatarUrl = avatarUrl;
							this.nickName = nickName;
						}
					} catch (e) {
						// error
						console.log('->err', e)
					}
				},
				fail: (fail) => {
					this.login = false;
					if (login) {
						uni.navigateTo({
							url: '/pages/Ahuixiang/login/login',
							animationType: 'pop-in',
							animationDuration: 200
						});
					}
				}
			});
		},
		loginUser() {
			// 需要登录
			uni.getUserInfo({
				lang: 'zh_CN',
				success: (res) => {
					// 判断登录状态否
					this.getUserInfo('login');
				},
				fail: (res) => {
					uni.navigateTo({
						url: '/pages/Ahuixiang/login/login',
						animationType: 'pop-in',
						animationDuration: 200
					});
				}
			})
		},
		selectMylist() {
			uni.request({
				url: `${getApp().globalData.URL}/clientUser/selectMylist`,
				method: 'GET',
				success: (res) => {
					this.lists = res.data;
				}
			})
		}
	},
	onPageScroll(e) {
		let scroll = e.scrollTop <= 0 ? 0 : e.scrollTop;
		let opcity = scroll / this.scrollH;
		if (this.opcity >= 1 && opcity >= 1) {
			return;
		}
		this.opcity = opcity;
		this.iconOpcity = 0.5 * (1 - opcity < 0 ? 0 : 1 - opcity)
	},
	onPullDownRefresh() {
		// 获取用户信息
		this.getUserInfo();
		setTimeout(() => {
			uni.stopPullDownRefresh()
		}, 200)
	},
	onReachBottom: function () {
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
	}
}
</script>

<style>
.tui-header-box {
	width: 100%;
	position: fixed;
	left: 0;
	top: 0;
	z-index: 9998;
}

.tui-header {
	width: 100%;
	font-size: 18px;
	line-height: 18px;
	font-weight: 500;
	height: 32px;
	display: flex;
	align-items: center;
	justify-content: center;
}

/* #ifndef MP */
.tui-header-icon {
	position: fixed;
	top: 0;
	right: 15px;
	display: flex;
	align-items: center;
	justify-content: space-between;
	height: 32px;
	transform: translateZ(0);
	z-index: 99999;
}

/* #endif */
/* #ifdef MP */
.tui-set-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
}

/* #endif */
.tui-icon-box {
	position: relative;
}

.tui-icon-setup {
	margin-left: 8rpx;
}

.tui-badge {
	position: absolute;
	font-size: 24rpx;
	height: 32rpx;
	min-width: 20rpx;
	padding: 0 6rpx;
	border-radius: 40rpx;
	right: 10rpx;
	top: -5rpx;
	transform: scale(0.8) translateX(60%);
	transform-origin: center center;
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 10;
}

.tui-badge-red {
	background: #f74d54;
	color: #fff;
}

.tui-badge-white {
	background: #fff;
	color: #f74d54;
}

.tui-badge-dot {
	position: absolute;
	height: 12rpx;
	width: 12rpx;
	border-radius: 50%;
	right: -12rpx;
	top: 0;
	background: #f74d54;
}

.tui-mybg-box {
	width: 100%;
	height: 464rpx;
	position: relative;
}

.tui-my-bg {
	width: 100%;
	height: 464rpx;
	display: block;
}

.tui-header-center {
	position: absolute;
	width: 100%;
	height: 128rpx;
	left: 0;
	top: 120rpx;
	padding: 0 30rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
}

.tui-avatar {
	flex-shrink: 0;
	width: 128rpx;
	height: 128rpx;
	border-radius: 128rpx;
	display: block;
}

.tui-info {
	width: 60%;
	padding-left: 30rpx;
}

.tui-nickname {
	font-size: 30rpx;
	font-weight: 500;
	color: #fff;
	display: flex;
	align-items: center;
}

.tui-img-vip {
	width: 56rpx;
	height: 24rpx;
	margin-left: 18rpx;
}

.tui-explain {
	width: 80%;
	font-size: 24rpx;
	font-weight: 400;
	color: #fff;
	opacity: 0.75;
	padding-top: 8rpx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.tui-btn-edit {
	flex-shrink: 0;
	padding-right: 22rpx;
}

.tui-header-btm {
	width: 100%;
	padding: 0 30rpx;
	box-sizing: border-box;
	position: absolute;
	left: 0;
	top: 280rpx;
	display: flex;
	align-items: center;
	justify-content: space-between;
	color: #fff;
}

.tui-btm-item {
	flex: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.tui-btm-num {
	font-size: 32rpx;
	font-weight: 600;
	position: relative;
}

.tui-btm-text {
	font-size: 24rpx;
	opacity: 0.85;
	padding-top: 4rpx;
}

.tui-content-box {
	width: 100%;
	padding: 0 30rpx;
	box-sizing: border-box;
	position: relative;
	top: -72rpx;
	z-index: 10;
}

.tui-box {
	width: 100%;
	background: #fff;
	box-shadow: 0 3rpx 20rpx rgba(183, 183, 183, 0.1);
	border-radius: 10rpx;
	overflow: hidden;
}

.tui-order-box {
	height: 208rpx;
}

.tui-cell-header {
	width: 100%;
	height: 74rpx;
	padding: 0 26rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: space-between;
}

.tui-cell-title {
	font-size: 30rpx;
	line-height: 30rpx;
	font-weight: 600;
	color: #333;
}

.tui-cell-sub {
	font-size: 26rpx;
	font-weight: 400;
	color: #999;
	padding-right: 28rpx;
}

.tui-order-list {
	width: 100%;
	height: 134rpx;
	padding: 0 30rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: space-between;
}

.tui-order-item {
	flex: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.tui-order-text,
.tui-tool-text {
	font-size: 26rpx;
	font-weight: 400;
	color: #666;
	padding-top: 4rpx;
}

.tui-tool-text {
	font-size: 24rpx;
}

.tui-order-icon {
	width: 56rpx;
	height: 56rpx;
	display: block;
}

.tui-assets-box {
	height: 178rpx;
	margin-top: 20rpx;
}

.tui-assets-list {
	height: 84rpx;
}

.tui-assets-num {
	font-size: 32rpx;
	font-weight: 500;
	color: #333;
	position: relative;
}

.tui-assets-text {
	font-size: 24rpx;
	font-weight: 400;
	color: #666;
	padding-top: 6rpx;
}

.tui-tool-box {
	margin-top: 20rpx;
}

.tui-flex-wrap {
	flex-wrap: wrap;
	height: auto;
	padding-bottom: 30rpx;
}

.tui-tool-item {
	width: 25%;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	padding-top: 30rpx;
}

.tui-tool-icon {
	width: 64rpx;
	height: 64rpx;
	display: block;
}

.tui-badge-icon {
	width: 66rpx;
	height: 30rpx;
	position: absolute;
	right: 0;
	transform: translateX(88%);
	top: -15rpx;
}

/*为你推荐*/
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
