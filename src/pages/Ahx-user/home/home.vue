<template>
	<view class="container">
		<!--tabbar-->
		<!-- <Menu :current="0"></Menu> -->
		<!--tabbar-->

		<!--header-->
		<view class="tui-header-banner">
			<!-- <view class="tui-rolling-news">
				<tui-icon name="news-fill" :size='28' color='#fff'></tui-icon>
				<swiper vertical autoplay circular interval="3000" class="tui-swiper">
					<swiper-item v-for="(item,index) in newsList" :key="index" class="tui-swiper-item">
						<view class="tui-news-item" @tap='detailLog'>{{item.title}}</view>
					</swiper-item>
				</swiper>
			</view> -->
			<!-- <view class="tui-hot-search">
				<view>热搜</view>
				<view class="tui-hot-tag" @tap="search">自热火锅</view>
				<view class="tui-hot-tag" @tap="search">华为手机</view>
				<view class="tui-hot-tag" @tap="search">有机酸奶</view>
				<view class="tui-hot-tag" @tap="search">苹果手机</view>
			</view> -->
			<view class="tui-banner-bg">
				<!--banner-->
				<view class="tui-banner-box">
					<swiper :indicator-dots="true" :autoplay="true" :interval="5000" :duration="150" class="tui-banner-swiper" :circular="true" indicator-color="rgba(255, 255, 255, 0.8)" indicator-active-color="#fff">
						<swiper-item v-for="(item,index) in banner" :key="index">
							<image :src="item" class="tui-slide-image" mode="scaleToFill" />
						</swiper-item>
					</swiper>
				</view>
			</view>
		</view>
		<!--header-->
		

		<view class="tui-product-category">
			<view style="position: absolute; bottom: -50rpx; width: 150rpx; height: 150rpx; border: solid 2rpx #ccc; left: 50rpx;">
				<image :src="info.logo_url" style="width: 100%; height: 100%;"></image>
			</view>
			<view style="margin-left: 220rpx; font-size: 36rpx;">
				{{info.title}}
			</view>
			<!-- <view class="tui-category-item" v-for="(item,index) in category" :key="index" :data-key="item.name" @tap="more($event, item.id)">
				<image :src="item.img_url" class="tui-category-img" mode="scaleToFill"></image>
				<view class="tui-category-name">{{item.name}}</view>
			</view>

			<view class="tui-category-item" @tap="classify">
				<tui-icon name="manage-fill" color="#2DC195" :size="40"></tui-icon>
				<view class="tui-category-name">更多分类</view>
			</view> -->
		</view>

		<!-- <view class="tui-product-box tui-pb-20 tui-bg-white">
			<view class="tui-group-name" @tap="more">
				<text>新人专享</text>
				<tui-icon name="arrowright" :size="20" color="#555"></tui-icon>
			</view>
			<view class="tui-activity-box" @tap="detail">
				<image src="/static/images/mall/activity/activity_1.jpg" class="tui-activity-img" mode="widthFix"></image>
				<image src="/static/images/mall/activity/activity_2.jpg" class="tui-activity-img" mode="widthFix"></image>
			</view>
		</view> -->
<!-- 
		<view class="tui-product-box tui-pb-20 tui-bg-white">
			<view class="tui-group-name" @tap="moreMerchant">
				<text>平台自营</text>
				<tui-icon name="arrowright" :size="20" color="#555"></tui-icon>
			</view>
			<view class="tui-new-box">
				<view class="tui-new-item" :class="[index!=0 && index!=1 ?'tui-new-mtop':'']" v-for="(item,index) in newProduct" :key="index" @tap="detail(item.id)">
					<image :src="'/static/images/mall/new/'+(item.type==1?'new':'discount')+'.png'" class="tui-new-label" v-if="item.isLabel"></image>
					<view class="tui-title-box">
						<view class="tui-new-title">{{item.title}}</view>
						<view class="tui-new-price">
							<text class="tui-new-present">￥{{Number(item.price) / 100}}</text>
							<text class="tui-new-original">￥{{Number(item.original_price) / 100}}</text>
						</view>
					</view>
					<image :src="JSON.parse(item.img_url)[0]" class="tui-new-img"></image>
				</view>
			</view>
		</view> -->

		<view class="tui-product-box">
			<view class="tui-group-name">
				<text>全部商品</text>
			</view>
			<view class="tui-product-list">
				<view class="tui-product-container">
					<block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2!=0">
						<!--商品列表-->
						<view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail(item.id)">
							<image :src="JSON.parse(item.img_url)[0]" class="tui-pro-img" mode="widthFix" />
							<view class="tui-pro-content">
								<view class="tui-pro-tit">{{item.title}}</view>
								<view>
									<view class="tui-pro-price">
										<text class="tui-sale-price">￥{{Number(item.price) / 100}}</text>
										<text class="tui-factory-price">￥{{Number(item.original_price) / 100}}</text>
									</view>
									<view class="tui-pro-pay">{{item.sales_num}}人付款</view>
								</view>
							</view>
						</view>
						<!--商品列表-->
						<!-- <template is="productItem" data="{{item,index:index}}" /> -->
					</block>
				</view>
				<view class="tui-product-container">
					<block v-for="(item,index) in productList" :key="index" v-if="(index+1)%2==0">
						<!--商品列表-->
						<view class="tui-pro-item" hover-class="hover" :hover-start-time="150" @tap="detail(item.id)">
							<image :src="JSON.parse(item.img_url)[0]" class="tui-pro-img" mode="widthFix" />
							<view class="tui-pro-content">
								<view class="tui-pro-tit">{{item.title}}</view>
								<view>
									<view class="tui-pro-price">
										<text class="tui-sale-price">￥{{Number(item.price) / 100}}</text>
										<text class="tui-factory-price">￥{{Number(item.original_price) / 100}}</text>
									</view>
									<view class="tui-pro-pay">{{item.sales_num}}人付款</view>
								</view>
							</view>
						</view>
						<!--商品列表-->
						<!-- <template is="productItem" data="{{item,index:index}}" /> -->
					</block>
				</view>
			</view>
		</view>

		<!--加载loadding-->
		<tui-loadmore :visible="loadding" :index="totalPage" type="red"></tui-loadmore>
		<tui-nomore :visible="!pullUpOn" bgcolor="#f7f7f7"></tui-nomore>
		<!--加载loadding-->
		<view class="tui-safearea-bottom"></view>
	</view>
</template>
<script>
import tuiIcon from "@/components/icon/icon"
import tuiTag from "@/components/tag/tag"
import tuiLoadmore from "@/components/loadmore/loadmore"
import tuiNomore from "@/components/nomore/nomore"
import { Base64 } from 'js-base64'

export default {
	components: {
		tuiIcon,
		tuiTag,
		tuiLoadmore,
		tuiNomore,
	},
	data() {
		return {
			hotSearch: [],// 搜索记录 ok
			banner: [],// 轮播图 ok
			category: [],
			newProduct: [],// 平台推荐 ok
			productList: [],// 商品列表 ok
			pageIndex: 1,
			totalPage: 1,
			loadding: true,
			pullUpOn: true,
			newsList: [],// 公告ok
			id: null,
			info: {}
		}
	},
	methods: {
		detail: function (id) {
			uni.navigateTo({
				url: `/pages/Ahuixiang/productDetail/productDetail?id=${id}`
			})
		},
		detailLog(e) {
			// this.tui.toast('详情功能尚未完善~')
		},
		classify: function () {
			uni.navigateTo({
				url: '../goodsType/goodsType'
			})

		},
		more: function (e, id) {
			let key = e.currentTarget.dataset.key || "";
			uni.navigateTo({
				url: `../productList/productList?searchKey=${key}&type=type&type_id=${id}`
			})
		},
		moreMerchant: function () {
			uni.navigateTo({
				url: `../productList/productList?searchKey=平台自营&type=merchant`
			})
		},
		search: function () {
			uni.navigateTo({
				url: '../news-search/news-search'
			})
		},
		getInfo() {
			uni.request({
				url: `${getApp().globalData.URL}/getSetting/getInfo.merchant?id=${this.id}`,
				success: (res) => {
					// this.newsList = res.data.affiche ? res.data.affiche : [{ title: '暂无公告' }];
					this.banner = res.data.shop_url ? JSON.parse(res.data.shop_url) : [];
					this.info = res.data;
					// this.hotSearch = res.data.search ? res.data.search : [{ title: '暂无搜索记录！' }]
					// this.category = res.data.type ? res.data.type : [{ name: '暂无推荐' }]
				}
			})
		},
		getGoodsListPage(page) {
			if (this.pageIndex == 1) {
				this.loadding = true
				this.pullUpOn = true
				this.productList = []
			}
			uni.request({
				url: `${getApp().globalData.URL}/clientGoods/merchant.page?page=${page}&id=${this.id}`,
				success: (res) => {
					this.productList = res.data.data ? this.productList.concat(res.data.data) : [];
					if (this.productList.length == 0) {
						this.pullUpOn = false
					}
					this.totalPage = res.data.totalPage;
					this.loadding = false
				}
			})
		},
		getGoodsMerchantList() {
			// getGoodsList
			uni.request({
				url: `${getApp().globalData.URL}/clientGoods/getGoodsList`,
				success: (res) => {
					this.newProduct = res.data !== '暂无数据' ? res.data : []
				}
			})
		}
	},
	onShow() {
		console.log('->show')
		this.getInfo()
	},
	onLoad(options) {
		console.log('->load')
		// 存储到访者来源
		if (options.path) {
			uni.setStorageSync('path', options.path);
		}

		console.log('->options', options.id)
		this.id = Number(options.id)

		// this.getGoodsMerchantList()
		this.pageIndex = 1;
		this.getGoodsListPage(this.pageIndex)
	},
	onShareAppMessage(res) {
		// 分享带分享者id
		let title = '惠享优选商城'
		let path = `/pages/Ahx-user/home/home?id=${this.id}`

		try {
			const value = uni.getStorageSync('token');
			if (value) {
				let id = JSON.parse(value).id;
				return { title, path: `${path}?path=${Base64.encode(id)}` }
			} else {
				return { title, path }
			}
		} catch (e) {
			return { title, path }
		}
	},
	//页面相关事件处理函数--监听用户下拉动作
	onPullDownRefresh: function () {
		// this.newsList = this.dataSources;
		this.currentTab = 0;
		this.pageIndex = 1;
		this.pullUpOn = true;
		this.loadding = true;
		this.getGoodsMerchantList()
		this.getGoodsListPage(this.pageIndex);
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
	// 页面上拉触底事件的处理函数
	onReachBottom: function () {
		if (!this.pullUpOn) return;
		this.loadding = true;
		if (this.pageIndex == this.totalPage) {
			this.loadding = false;
			this.pullUpOn = false
		} else {
			this.pageIndex = this.pageIndex + 1;
			this.getGoodsListPage(this.pageIndex);
		}
	},
	onPageScroll(e) {
		this.scrollTop = e.scrollTop;
	},
}
</script>

<style>
page {
	background: #f7f7f7;
}

.container {
	padding-bottom: 100rpx;
	color: #333;
}

.tui-header {
	width: 100%;
	height: 50rpx;
	/* padding: 0 30rpx 0 20rpx; */
	box-sizing: border-box;
	/* background: #2DC195; */
	display: flex;
	align-items: center;
	justify-content: space-between;
	position: relative;
	/* position: fixed; */
	/* left: 0; */
	/* top: 0; */
	/* #ifdef H5 */
	/* top: 44px; */
	/* #endif */
	z-index: 999;
}

.tui-ms {
	width: 100%;
	height: 50rpx;
	position: absolute;
	display: flex;
	/* top: -155rpx; */
	overflow: hidden;
}

.tui-rolling-news {
	width: 100%;
	padding: 12upx 30upx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-wrap: nowrap;
}

.tui-news-item {
	line-height: 28upx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
	color: #fff;
}

.tui-rolling-search {
	position: absolute;
	bottom: -30rpx;
	width: calc(100% - 40rpx);
	margin: 0 30rpx 0 20rpx;
	height: 60rpx;
	border: #2dc195 solid 1px;
	border-radius: 10rpx;
	padding: 0 40rpx 0 30rpx;
	box-sizing: border-box;
	background: #fff;
	display: flex;
	align-items: center;
	flex-wrap: nowrap;
	color: #999;
	z-index: 99;
}

.tui-category {
	font-size: 24rpx;
	color: #fff;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	margin: 0;
	margin-right: 22rpx;
	flex-shrink: 0;
}

.tui-category-scale {
	transform: scale(0.7);
	line-height: 24rpx;
}

.tui-icon-category {
	line-height: 20px !important;
	margin-bottom: 0 !important;
}

.tui-swiper {
	font-size: 26rpx;
	height: 60rpx;
	flex: 1;
	padding-left: 12rpx;
}

.tui-swiper-item {
	display: flex;
	align-items: center;
}

.tui-hot-item {
	line-height: 26rpx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.tui-header-banner {
	/* padding-top: 100rpx; */
	box-sizing: border-box;
	/* background: #2dc195; */
	height: 260rpx;
	/* height: calc(260rpx + 64rpx); */
	position: relative;
	z-index: 1000;
}

.tui-hot-search {
	color: #fff;
	font-size: 24rpx;
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 0 20rpx;
	box-sizing: border-box;
	position: relative;
	z-index: 2;
}

.tui-hot-tag {
	background: rgba(255, 255, 255, 0.15);
	padding: 10rpx 24rpx;
	border-radius: 30rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	line-height: 24rpx;
	/* margin-left: 20rpx; */
}

.tui-banner-bg {
	display: flex;
	height: 180rpx;
	/* background: #2dc195; */
	position: relative;
}

.tui-primary-bg {
	width: 50%;
	display: inline-block;
	height: 224rpx;
	border: 1px solid transparent;
	position: relative;
	top: -155rpx;
	z-index: 1;
	background: #2dc195;
}

.tui-route-left {
	transform: skewY(8deg);
}

.tui-route-right {
	transform: skewY(-8deg);
}

.tui-banner-box {
	width: 100%;
	padding: 0 20rpx;
	box-sizing: border-box;
	position: absolute;
	/* overflow: hidden; */
	z-index: 99;
	/* bottom: -80rpx; */
	left: 0;
}

.tui-banner-swiper {
	width: 100%;
	height: 240rpx;
	border-radius: 12rpx;
	overflow: hidden;
	transform: translateY(0);
}

.tui-slide-image {
	width: 100%;
	height: 240rpx;
	display: block;
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
.tui-banner-swiper .uni-swiper-dot {
	width: 8rpx;
	height: 8rpx;
	display: inline-flex;
	background: none;
	justify-content: space-between;
}

.tui-banner-swiper .uni-swiper-dot::before {
	content: '';
	flex-grow: 1;
	background: rgba(255, 255, 255, 0.8);
	border-radius: 16rpx;
	overflow: hidden;
}

.tui-banner-swiper .uni-swiper-dot-active::before {
	background: #fff;
}

.tui-banner-swiper .uni-swiper-dot.uni-swiper-dot-active {
	width: 16rpx;
}

/* #endif */

.tui-product-category {
	background: #fff;
	padding: 40rpx 20rpx 30rpx 20rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	justify-content: flex-start;
	flex-wrap: wrap;
	font-size: 24rpx;
	color: #555;
	position: relative;
	/* margin-bottom: 20rpx; */
}

.tui-category-item {
	width: 20%;
	height: 118rpx;
	display: flex;
	align-items: center;
	justify-content: space-between;
	flex-direction: column;
	padding-top: 30rpx;
}

.tui-category-img {
	height: 80rpx;
	width: 80rpx;
	display: block;
}

.tui-category-name {
	line-height: 24rpx;
}

.tui-product-box {
	margin-top: 40rpx;
	padding: 0 20rpx;
	box-sizing: border-box;
}

.tui-pb-20 {
	padding-bottom: 20rpx;
}

.tui-bg-white {
	background: #fff;
}

.tui-group-name {
	font-size: 36rpx;
	font-weight: bold;
	text-align: center;
	padding: 26rpx 0;
}

.tui-activity-box {
	display: flex;
	border-radius: 12rpx;
	overflow: hidden;
}

.tui-activity-img {
	width: 50%;
	display: block;
}

.tui-new-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
	flex-wrap: wrap;
}

.tui-new-item {
	width: 49%;
	height: 200rpx;
	padding: 0 20rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	background: #f5f2f9;
	position: relative;
	border-radius: 12rpx;
}

.tui-new-mtop {
	margin-top: 2%;
}

.tui-title-box {
	font-size: 24rpx;
}

.tui-new-title {
	line-height: 32rpx;
	word-break: break-all;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
}

.tui-new-price {
	padding-top: 18rpx;
}

.tui-new-present {
	color: #ff201f;
	font-weight: bold;
}

.tui-new-original {
	display: inline-block;
	color: #a0a0a0;
	text-decoration: line-through;
	padding-left: 12rpx;
	transform: scale(0.8);
	transform-origin: center center;
}

.tui-new-img {
	width: 160rpx;
	height: 160rpx;
	display: block;
	flex-shrink: 0;
}

.tui-new-label {
	width: 56rpx;
	height: 56rpx;
	border-top-left-radius: 12rpx;
	position: absolute;
	left: 0;
	top: 0;
}

.tui-product-list {
	display: flex;
	justify-content: space-between;
	flex-direction: row;
	flex-wrap: wrap;
	box-sizing: border-box;
	/* padding-top: 20rpx; */
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
