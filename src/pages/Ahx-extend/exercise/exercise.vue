<template>
	<view class="container">
		<Menu :current="1"></Menu>

		<!--banner-->
		<view class="tui-banner-box">
			<swiper :indicator-dots="true" :autoplay="true" :interval="5000" :duration="150" class="tui-banner-swiper" :circular="true" previous-margin="60rpx" next-margin="60rpx" @change="change">
				<swiper-item v-for="(item,index) in goodsImage" :key="index" class="tui-banner-item">
					<image :src="item.img_url" class="tui-slide-image" :class="[current!=index?'tui-banner-scale':'']" mode="scaleToFill" @tap="detailImg(item)" />
				</swiper-item>
			</swiper>
		</view>
		<!--banner-->

		<!--headlines-->
		<!-- <view class="tui-rolling-news list-item">
			<tui-icon name="news" :size='22' color='#555'></tui-icon>
			<swiper :vertical="true" :autoplay="true" :circular="true" :interval="4000" class="tui-swiper">
				<swiper-item v-for="(item,index) in headlines" :key="index" class="tui-swiper-item">
					<view class="tui-news-item">{{item}}</view>
				</swiper-item>
			</swiper>
		</view> -->
		<!--headlines-->
		<!--classify-->
		<!-- <view class="tui-classify-box">
			<view class="tui-classify-item" v-for="(item,index) in classify" :key="index" @tap="more" :data-key="item.name">
				<image :src="'../../../static/images/classify/'+item.img+'.png'" class="tui-classify-img" />
				<view class="tui-classify-name">{{item.name}}</view>
			</view>
		</view> -->
		<!--classify-->

		<view class="tui-box tui-tool-box">
			<view class="tui-order-list tui-flex-wrap">
				<view @tap="pathExercise(1)" class="tui-tool-item">
					<view class="tui-icon-box">
						<image src="/static/images/mall/my/icon_zhihuan_3x.png" class="tui-tool-icon"></image>
					</view>
					<view class="tui-tool-text">砍价活动</view>
				</view>

				<view @tap="pathExercise(2)" class="tui-tool-item">
					<view class="tui-icon-box">
						<image src="/static/images/mall/my/icon_tuan_3x.png" class="tui-tool-icon"></image>
					</view>
					<view class="tui-tool-text">超值拼团</view>
				</view>

				<view @tap="pathExercise(3)" class="tui-tool-item">
					<view class="tui-icon-box">
						<image src="/static/images/mall/my/icon_fanxian_3x.png" class="tui-tool-icon"></image>
					</view>
					<view class="tui-tool-text">限时秒杀</view>
				</view>

				<view @tap="pathExercise(4)" class="tui-tool-item">
					<view class="tui-icon-box">
						<image src="/static/images/mall/my/icon_gift_3x.png" class="tui-tool-icon"></image>
						<!-- <image src="/static/images/mall/my/icon_tab_3x.png" class="tui-badge-icon"></image> -->
					</view>
					<view class="tui-tool-text">积分兑换</view>
				</view>
			</view>
		</view>

		<view class="product-list tui-box tui-tool-box">
			<view class="tui-cell-header" @tap="pathExercise(1)">
				<view class="tui-cell-title">砍价专场</view>
				<view class="tui-cell-sub">查看更多
					<tui-icon name="arrowright" :size="20" color="#999"></tui-icon>
				</view>
			</view>
			<!--商品列表-->
			<view class="pro-item" @tap="detail(item.id, 'hx_exercise_bargain')" v-for="(item,index) of goodsList.hx_exercise_bargain" :key="index">
				<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="pro-img" mode="widthFix" />
				<view class="pro-content">
					<view class="pro-tit">{{item.hx_goods.title}}</view>
					<view>
						<view class="pro-price">
							<text class="sale-price">
								<text style="font-size: 24rpx">可砍至</text>￥{{item.end_price / 100}}</text>
							<text class="factory-price">￥{{item.hx_goods.original_price / 100}}</text>
						</view>
						<view class="pro-pay">{{item.hx_goods.sales_num}}人付款</view>
					</view>
				</view>
			</view>
			<!--商品列表-->
		</view>

		<view class="product-list tui-box tui-tool-box">
			<view class="tui-cell-header" @tap="pathExercise(2)">
				<view class="tui-cell-title">拼团专场</view>
				<view class="tui-cell-sub">查看更多
					<tui-icon name="arrowright" :size="20" color="#999"></tui-icon>
				</view>
			</view>
			<!--商品列表-->
			<view class="pro-item" @tap="detailFight(item.id, 'hx_exercise_fight')" v-for="(item,index) of goodsList.hx_exercise_fight" :key="index">
				<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="pro-img" mode="widthFix" />
				<view class="pro-content">
					<view class="pro-tit">{{item.hx_goods.title}}</view>
					<view>
						<view class="pro-price">
							<text class="sale-price">
								<text style="font-size: 24rpx">拼团价</text>¥{{item.price / 100}}</text>
							<text class="factory-price">原价{{item.original_price / 100}}</text>
						</view>
						<view class="pro-pay">最低{{item.fight_num}}人成团</view>
						<view class="pro-pay">{{item.hx_goods.sales_num}}人付款</view>
					</view>
				</view>
			</view>
			<!--商品列表-->
		</view>

		<view class="product-list tui-box tui-tool-box">
			<view class="tui-cell-header" @tap="pathExercise(3)">
				<view class="tui-cell-title">秒杀专场</view>
				<view class="tui-cell-sub">查看更多
					<tui-icon name="arrowright" :size="20" color="#999"></tui-icon>
				</view>
			</view>
			<!--商品列表-->
			<view class="pro-item" @tap="detailTimeLimit(item.id, 'hx_exercise_time_limit')" v-for="(item,index) of goodsList.hx_exercise_time_limit" :key="index">
				<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="pro-img" mode="widthFix" />
				<view class="pro-content">
					<view class="pro-tit">{{item.hx_goods.title}}</view>
					<view>
						<view class="pro-price">
							<text class="sale-price">
								<text style="font-size: 24rpx">秒杀价</text>¥{{item.price / 100}}</text>
							<text class="factory-price">原价{{item.original_price / 100}}</text>
						</view>
						<view class="pro-pay">{{times(item).text}}</view>
						<tui-countdown :time="times(item).time" color="#fff" bcolor="#e41f19" bgcolor="#e41f19" colonColor="#e41f19" @end="endOfTime"></tui-countdown>
						<view class="pro-pay">只剩{{item.stock_num}}件</view>
						<view class="pro-pay">{{item.hx_goods.sales_num}}人付款</view>
					</view>
				</view>
			</view>
			<!--商品列表-->
		</view>

		<view class="product-list tui-box tui-tool-box">
			<view class="tui-cell-header" @tap="pathExercise(4)">
				<view class="tui-cell-title">阳光币兑换</view>
				<view class="tui-cell-sub">查看更多
					<tui-icon name="arrowright" :size="20" color="#999"></tui-icon>
				</view>
			</view>
			<!--商品列表-->
			<view class="pro-item" @tap="detailPoints(item.id, 'hx_exercise_points')" v-for="(item,index) of goodsList.hx_exercise_points" :key="index">
				<image :src="JSON.parse(item.hx_goods.img_url)[0]" class="pro-img" mode="widthFix" />
				<view class="pro-content">
					<view class="pro-tit">{{item.hx_goods.title}}</view>
					<view>
						<view class="pro-price">
							<text class="sale-price">
								<text style="font-size: 24rpx">阳光币:</text>{{item.points}}</text>
							<text class="factory-price">价格{{item.hx_goods.original_price / 100}}</text>
						</view>
						<view class="pro-pay">{{item.hx_goods.sales_num}}人付款</view>
					</view>
				</view>
			</view>
			<!--商品列表-->
		</view>

		<!-- <tui-sticky :scrollTop="scrollTop" stickyHeight="104rpx" class="tui-box tui-tool-box">
			<template v-slot:header>
				<tui-list-cell :arrow="true" padding="0" :lineLeft="false">
					<view class="tui-cell-header">
						<view class="tui-cell-title">积分商城</view>
						<view class="tui-cell-sub">查看更多</view>
					</view>
				</tui-list-cell>
			</template>
			<template v-slot:content class="tui-order-list tui-flex-wrap">
				<view class="product-list">
					<view class="pro-item" @tap="detail" v-for="(item,index) of 6" :key="index">
						<image :src="'../../../static/images/product/1.jpg'" class="pro-img" mode="widthFix" />
						<view class="pro-content">
							<view class="pro-tit">11</view>
							<view>
								<view class="pro-price">
									<text class="sale-price">￥22</text>
									<text class="factory-price">￥15</text>
								</view>
								<view class="pro-pay">2人付款</view>
							</view>
						</view>
					</view>
				</view>
			</template>
		</tui-sticky> -->

	</view>
</template>

<script>
import tuiIcon from "@/components/icon/icon"
import tuiCountdown from "@/components/countdown/countdown"
import Menu from '../../Ahuixiang/menu/menu.vue'
import tuiListCell from "@/components/list-cell/list-cell"
import tuiSticky from "@/components/sticky/sticky"
import tuiToast from "@/components/extend/toast/toast"
import { Base64 } from 'js-base64'

export default {
	components: {
		tuiIcon,
		tuiCountdown,
		Menu,
		tuiListCell,
		tuiSticky,
		tuiToast
	},
	data() {
		return {
			scrollTop: 0,
			goodsList: {
				hx_exercise_fight: [],
				hx_exercise_bargain: [],
				hx_exercise_time_limit: [],
				hx_exercise_points: [],
				hx_exercise_image: []
			},
			banner: [
				"1.jpg",
				"2.jpg",
				"3.jpg",
				"4.jpg",
				"5.jpg"
			],
			classify: [{
				img: "kongtiao",
				name: "空调"
			},
			{
				img: "Icewash",
				name: "冰洗"
			},
			{
				img: "heater",
				name: "热水器"
			},
			{
				img: "bed",
				name: "床"
			},
			{
				img: "boutique",
				name: "精品"
			}
			],
			current: 0,
			headlines: [
				"苹果XR对比华为Mate20你会选择谁",
				"格兰仕暗示拜访拼多多后遭天猫打压，拼多多高层赞其有勇气",
				"耐克没进前十，今年Q1全球受欢迎品牌榜"
			],
			productList: [
				{
					img: 1,
					name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜 30ml（欧莱雅彩妆 BB霜 粉BB 遮瑕疵 隔离）",
					sale: 599,
					factory: 899,
					time: 2000
				},
				{
					img: 2,
					name: "德国DMK进口牛奶  欧德堡（Oldenburger）超高温处理全脂纯牛奶1L*12盒",
					sale: 29,
					factory: 69,
					time: 1500
				},
				{
					img: 3,
					name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
					sale: 299,
					factory: 699,
					time: 1800
				},
				{
					img: 4,
					name: "百雀羚套装女补水保湿护肤品",
					sale: 1599,
					factory: 2899,
					time: 1000
				},
				{
					img: 5,
					name: "百草味 肉干肉脯 休闲零食 靖江精制猪肉脯200g/袋",
					sale: 599,
					factory: 899,
					time: 3000
				},
				{
					img: 6,
					name: "短袖睡衣女夏季薄款休闲家居服短裤套装女可爱韩版清新学生两件套 短袖粉色长颈鹿 M码75-95斤",
					sale: 599,
					factory: 899,
					time: 2400
				},
				{
					img: 1,
					name: "欧莱雅（LOREAL）奇焕光彩粉嫩透亮修颜霜",
					sale: 599,
					factory: 899,
					time: 1600
				},
				{
					img: 2,
					name: "德国DMK进口牛奶",
					sale: 29,
					factory: 69,
					time: 1200
				},
				{
					img: 3,
					name: "【第2支1元】柔色尽情丝柔口红唇膏女士不易掉色保湿滋润防水 珊瑚红",
					sale: 299,
					factory: 699,
					time: 1800
				},
				{
					img: 4,
					name: "百雀羚套装女补水保湿护肤品",
					sale: 1599,
					factory: 2899,
					time: 2600
				}
			],
			goodsImage: []
		}
	},
	onLoad(options) {
		// 存储到访者来源
		if (options.path) {
			uni.setStorageSync('path', options.path);
		}
		this.getExerciseGoodsList();
	},
	onShareAppMessage(res) {
		// 分享带分享者id
		let title = '活动中心'
		let path = '/pages/Ahx-extend/exercise/exercise'

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
	computed: {
		times: function () {
			return function (item) {
				let time = item.times;
				let text = '距离结束：'
				if (item.subtract_times >= 0) {
					text = '距离开始：'
					time = item.subtract_times
				}
				if (time < 0) text = '秒杀结束：'
				return { time, text }
			}
		}
	},
	methods: {
		pathExercise(index) {
			let path = {
				1: `../productList/productList?searchKey=砍价专场&type=bargain`,
				2: `../productList/productList?searchKey=拼团专场&type=fight`,
				3: `../productList/productList?searchKey=秒杀专场&type=time`,
				4: `../productList/productList?searchKey=阳光币专场&type=points`,
			}
			uni.navigateTo({ url: path[index] })
		},
		endOfTime: function () {
			// this.tui.toast("砍价结束，请尽快下单！")
		},
		change: function (e) {
			this.current = e.detail.current
		},
		detail: function (id, type) {
			uni.navigateTo({
				url: `./productDetail?id=${id}&type=${type}`
			})
		},
		detailImg(item) {
			console.log('->item', item)
			let path = {
				1: `./productDetail?id=${item.goods_id}&type=hx_exercise_bargain`,
				2: `./productDetail.fight?id=${item.goods_id}&type=hx_exercise_fight`,
				3: `./productDetail.time?id=${item.goods_id}&type=hx_exercise_time_limit`,
				4: `./productDetail.points?id=${item.goods_id}&type=hx_exercise_points`
			}
			uni.navigateTo({ url: path[item.type] })
		},
		detailFight(id, type) {
			uni.navigateTo({
				url: `./productDetail.fight?id=${id}&type=${type}`
			})
		},
		detailTimeLimit(id, type) {
			uni.navigateTo({
				url: `./productDetail.time?id=${id}&type=${type}`
			})
		},
		detailPoints(id, type) {
			uni.navigateTo({
				url: `./productDetail.points?id=${id}&type=${type}`
			})
		},
		more: function (e) {
			let key = e.currentTarget.dataset.key || "";
			uni.navigateTo({
				url: '/pages/Ahuixiang/productList/productList?searchKey=' + key
			})
		},
		getExerciseGoodsList() {
			uni.request({
				url: `${getApp().globalData.URL}/clientExercise/getExerciseGoodsList`,
				success: (res) => {
					this.goodsList = res.data;
				}
			})
			// 轮播图
			uni.request({
				url: `${getApp().globalData.URL}/clientExercise/getExerciseGoodsImage`,
				success: (res) => {
					this.goodsImage = res.data != '暂无数据' ? res.data : []
				}
			})
		}
	},
	//页面滚动执行方式
	onPageScroll(e) {
		this.scrollTop = e.scrollTop
		// console.log('->this.scrollTop', this.scrollTop)
	},
	onShow() {

	},
	//页面相关事件处理函数--监听用户下拉动作
	onPullDownRefresh: function () {
		// this.newsList = this.dataSources;
		this.currentTab = 0;
		this.pageIndex = 1;
		this.pullUpOn = true;
		this.loadding = true;
		this.getExerciseGoodsList();
		uni.stopPullDownRefresh();
		// let options = {
		// 	msg: "刷新成功!",
		// 	duration: 2000,
		// 	type: "translucent"
		// };
		// setTimeout(() => {
		// 	this.$refs.toast.showTips(options);
		// }, 300);
	},
}
</script>

<style>
.container {
	padding-bottom: 100rpx;
	box-sizing: border-box;
}

/*banner*/

.tui-banner-box {
	width: 100%;
	padding-top: 20rpx;
	box-sizing: border-box;
	background: #fff;
	/* margin-bottom: 20rpx; */
}

.tui-banner-swiper {
	width: 100%;
	height: 240rpx;
}

.tui-banner-item {
	padding: 0 16rpx;
	box-sizing: border-box;
}

.tui-slide-image {
	width: 100%;
	height: 240rpx;
	display: block;
	border-radius: 12rpx;
	/* transition: all 0.1s linear; */
}

.tui-banner-scale {
	transform: scaleY(0.9);
	transform-origin: center center;
}

/* #ifdef APP-PLUS || MP */
.tui-banner-swiper .wx-swiper-dot {
	width: 8rpx;
	height: 8rpx;
	display: inline-flex;
	background: none;
	justify-content: space-between;
}

.tui-banner-swiper .wx-swiper-dot::before {
	content: '';
	flex-grow: 1;
	background: rgba(255, 255, 255, 0.8);
	border-radius: 16rpx;
	overflow: hidden;
}

.tui-banner-swiper .wx-swiper-dot-active::before {
	background: #fff;
}

.tui-banner-swiper .wx-swiper-dot.wx-swiper-dot-active {
	width: 16rpx;
}

/* #endif */

/* #ifdef H5 */
>>> .tui-banner-swiper .uni-swiper-dot {
	width: 8rpx;
	height: 8rpx;
	display: inline-flex;
	background: none;
	justify-content: space-between;
}

>>> .tui-banner-swiper .uni-swiper-dot::before {
	content: '';
	flex-grow: 1;
	background: rgba(255, 255, 255, 0.8);
	border-radius: 16rpx;
	overflow: hidden;
}

>>> .tui-banner-swiper .uni-swiper-dot-active::before {
	background: #fff;
}

>>> .tui-banner-swiper .uni-swiper-dot.uni-swiper-dot-active {
	width: 16rpx;
}

/* #endif */
/*banner*/

/*headlines*/

.tui-rolling-news {
	width: 100%;
	padding: 0 30rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-wrap: nowrap;
	background: #fff;
}

.tui-rolling-news::after {
	left: 0;
}

.tui-swiper {
	margin-left: 8rpx;
	font-size: 28rpx;
	height: 80rpx;
	flex: 1;
}

.tui-swiper-item {
	display: flex;
	align-items: center;
}

.tui-news-item {
	line-height: 28rpx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
	color: #555;
}

/*headlines*/

/*classify*/

.tui-classify-box {
	width: 100%;
	display: flex;
	flex-wrap: wrap;
	align-items: center;
	justify-content: space-between;
	padding: 28rpx 0;
	background-color: #fff;
}

.tui-classify-item {
	width: 20%;
	text-align: center;
}

.tui-classify-img {
	width: 88rpx;
	height: 88rpx;
}

.tui-classify-name {
	font-size: 26rpx;
	line-height: 26rpx;
	padding-top: 8rpx;
	color: #555;
	white-space: nowrap;
}

/*classify*/

/*spike*/

.tui-spike-box {
	background: #fff;
	margin-top: 20rpx;
}

.tui-spike-title {
	padding: 20rpx 30rpx;
	box-sizing: border-box;
	font-size: 30rpx;
	color: #333;
	font-weight: bold;
}

.tui-spike-title::after {
	left: 0;
}

.tui-spike-swiper {
	min-height: 440rpx;
}

.tui-pro-item {
	display: flex;
	width: 100%;
	background: #fff;
	box-sizing: border-box;
	border-radius: 12rpx;
	position: relative;
}

.tui-pro-item::after {
	left: 240rpx;
}

.tui-pro-img {
	width: 220rpx;
	height: 220rpx;
	display: block;
	flex-shrink: 0;
	border-radius: 12rpx;
}

.tui-pro-content {
	flex: 1;
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

.tui-pro-btmbox {
	width: 100%;
	display: flex;
	align-items: center;
	justify-content: space-between;
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

.tui-countdown {
	display: flex;
	align-items: center;
}

.tui-countdown-text {
	padding: 0 8rpx;
	font-size: 24rpx;
	line-height: 24rpx;
	color: #555;
}
.product-list {
	display: flex;
	display: -webkit-flex;
	justify-content: space-between;
	flex-direction: row;
	flex-wrap: wrap;
	box-sizing: border-box;
	padding: 10upx 30upx 30upx 30upx;
	background: #fff;
}

.pro-item {
	background: #fff;
	box-sizing: border-box;
	width: 49%;
	position: relative;
	margin-bottom: 2%;
}

.pro-item::after {
	content: '';
	position: absolute;
	height: 200%;
	width: 200%;
	border: 1upx solid #eaeef1;
	transform-origin: 0 0;
	-webkit-transform-origin: 0 0;
	-webkit-transform: scale(0.5);
	transform: scale(0.5);
	left: 0;
	top: 0;
}

.pro-img {
	width: 100%;
	display: block;
}

.pro-content {
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	box-sizing: border-box;
	padding: 20upx;
	min-height: 230upx;
	height: auto;
}

.pro-tit {
	/* font-size:  */
	color: #2e2e2e;
	font-size: 26upx;
	word-break: break-all;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
}

.pro-price {
	padding-top: 18upx;
}

.sale-price {
	font-size: 34upx;
	font-weight: 500;
	color: #ea1500;
}

.factory-price {
	font-size: 24upx;
	color: #a0a0a0;
	text-decoration: line-through;
	padding-left: 12upx;
}

.pro-pay {
	padding-top: 10upx;
	font-size: 24upx;
	color: #656565;
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
	height: 94rpx;
	padding: 30rpx 0rpx 10rpx 0rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: space-between;
	border-bottom: solid 1px #f9f9f9;
	margin-bottom: 30rpx;
}

.tui-cell-title {
	font-size: 38rpx;
	line-height: 38rpx;
	font-weight: 600;
	color: #333;
}

.tui-cell-sub {
	font-size: 30rpx;
	font-weight: 400;
	color: #999;
	/* padding-right: 28rpx; */
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
	padding-bottom: 25rpx;
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
/*spike*/
</style>
