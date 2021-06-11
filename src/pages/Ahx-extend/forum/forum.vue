<template>
    <view class="tui-container">
        <Menu :current="3"></Menu>

        <tui-fab :width="70" :height="70" :left="left" :right="right" :bottom="bottom" :bgColor="bgColor" :btnList="btnList" @click="onClick"></tui-fab>

        <view class='tui-notice-board'>
            <view class="tui-icon-bg">
                <tui-icon name="news-fill" :size='24' color='#f54f46'></tui-icon>
            </view>
            <view class="tui-scorll-view" @tap='detail'>
                <view class="tui-notice" :class="[animation?'tui-animation':'']">
                    <text v-for="(item, index) in getBulletin" :key="index">{{item.title}}</text>
                </view>
            </view>
        </view>


        <!--banner-->
        <swiper indicator-dots autoplay circular :interval="5000" :duration="150" indicator-color="rgba(255, 255, 255, 0.9)" indicator-active-color="#5677fc" class="tui-banner-swiper">
            <swiper-item v-for="(item,index) in getImage" :key="index" @tap.stop="bannerDetail(item.forum_id)">
                <view class="tui-banner-title">{{item.name}}</view>
                <image :src="item.img_url" class="tui-slide-image" mode="widthFix" />
            </swiper-item>
        </swiper>


        <view class="tui-searchboxeasy">
            <view class="tui-search-input">
                <!-- #ifdef APP-PLUS || MP -->
                <icon type="search" :size='13' color='#333'></icon>
                <!-- #endif -->
                <!-- #ifdef H5 -->
                <view>
                    <tui-icon name="search" :size='16' color='#333'></tui-icon>
                </view>
                <!-- #endif -->
                <input confirm-type="search" :placeholder="hot.length != 0 ? '大家都在搜：' + hot[0] : '大家都在搜：慧享优选'"  placeholder-class="tui-input-plholder" class="tui-input" v-model.trim="key" @confirm="searchClick(1)" />
                <!-- #ifdef APP-PLUS || MP -->
                <icon type="clear" :size='13' color='#bcbcbc' @tap="cleanKey" v-show="key"></icon>
                <!-- #endif -->
                <!-- #ifdef H5 -->
                <view @tap="cleanKey" v-show="key">
                    <tui-icon name="close-fill" :size='16' color='#bcbcbc'></tui-icon>
                </view>
                <!-- #endif -->
            </view>
        </view>


        <view class="ul">
            <!-- <view class="li mor_active">杂谈</view> -->
            <view class="li" v-for="(item, index) in getType" :key="index" :class="index == getTypeIndex ? ' mor_active': ''" @tap="selectType(item.id, index)">{{item.title}}</view>
        </view>

        <!-- <tui-grid>
			<block v-for="(item,index) in getType" :key="index">
				<tui-grid-item cell="4" @click="selectType(item.id)">
					<view class="tui-grid-icon">
						<image v-if="item.img_url" :src="item.img_url" style="width: 60rpx; height: 60rpx;"></image>
					</view>
					<text class="tui-grid-label">{{item.title}}</text>
				</tui-grid-item>
			</block>
		</tui-grid> -->

        <!--新闻列表-->
        <view class="tui-news-view">
            <block v-for="(item,index) in newsList" :key="index">
                <tui-list-cell :index="index" @click="detail" :last="count==index">
                    <view class="tui-news-flex" :class="[item.isVideo || item.img_url.length > 1 ?  'tui-flex-column':'tui-flex-start']">
                        <!-- <view class="tui-news-picbox" :class="[item.isVideo || item.img_url.length > 1 ?'tui-w-full':'tui-w220 tui-h165', item.img_url.length > 1 ? 'tui-flex-between':'']"
                        v-if="item.img_url.length > 0">
							
                            <block v-for="(items,index2) in item.img_url" :key="index2">
                                <image :src="items" mode="widthFix" class="tui-block" :class="[item.img_url.length>1?'tui-one-third':'',item.isVideo?'tui-w-full':'']"></image>
                            </block>
                            <view class="tui-btm-badge" v-if="item.isVideo || item.img_url.length > 3">{{item.isVideo?item.time:item.img_url.length+'图'}}</view>
                            <view class="tui-video" v-if="item.isVideo">
                                <tui-icon name="play" color="#fff" :size="24"></tui-icon>
                            </view>
                        </view> -->
                        <view class="tui-news-tbox tui-flex-column tui-flex-between" :class="[item.imgNum===1 && !item.isVideo?'tui-h165 tui-pl-20':'']">
                            <view class="tui-news-title" :class="[(!item.isVideo && item.imgNum===1)|| item.imgNum===0?'':'tui-pt20']">{{item.title}}</view>
                            <view class="tui-sub-box" :class="[!item.isVideo && item.imgNum===1?'':'tui-pt20']">
                                <view class="tui-sub-source">
                                    <image style="float:left" :src="item.hx_user.img_url" class="tui-acatar"></image>
                                    <view style="float:left; height: 30rpx; line-height: 30rpx;">{{item.hx_user.name}}</view>
                                </view>
                                <view class="tui-sub-cmt">
                                    <view>
                                        <text style="padding-right: 30rpx">{{item.time_at}}</text>
                                        {{item.comment_num}}评论
                                    </view>
                                    <view class="tui-scale">
                                        <tui-tag size="small" :plain="true" shape="circleRight" v-if="item.status">置顶</tui-tag>
                                    </view>
                                </view>
                            </view>
                        </view>
                    </view>
                </tui-list-cell>
            </block>

            <view style="margin-top: 30rpx; text-align: center; color: #ddd" v-if="newsListStatus">暂无数据</view>

        </view>
        <tui-tips ref="toast"></tui-tips>
        <!--加载loadding-->
        <tui-loadmore :visible="loadding" :index="totalPage" type="primary"></tui-loadmore>
        <tui-nomore style="margin-bottom: 100rpx;" :visible="!pullUpOn" bgcolor="#f2f2f2"></tui-nomore>
        <!--加载loadding-->
    </view>
</template>

<script>
import tuiIcon from "@/components/icon/icon"
import tuiTag from "@/components/tag/tag"
import tuiListCell from "@/components/list-cell/list-cell"
import tuiLoadmore from "@/components/loadmore/loadmore"
import tuiNomore from "@/components/nomore/nomore"
import tuiTips from "@/components/tips/tips"
import Menu from "../../Ahuixiang/menu/menu.vue"
import tuiGrid from "@/components/grid/grid"
import tuiGridItem from '@/components/grid-item/grid-item'
import tuiFab from "@/components/tui-fab/tui-fab"
import { Base64 } from 'js-base64'

export default {
    components: {
        tuiIcon,
        tuiTag,
        tuiListCell,
        tuiLoadmore,
        tuiNomore,
        tuiTips,
        tuiGrid,
        tuiGridItem,
        tuiFab,
        Menu
    },
    computed: {
        count() {
            return this.newsList.length - 1
        }
    },
    data() {
        return {
            hotSearch: [
                "早安D站",
                "2019退役球星",
                "卡拉斯科"
            ],
            banner: [{
                img: "banner_1.jpg",
                title: "山东官方：德尔加多已完成全部手续办理，具备上场比赛资格"
            }, {
                img: "banner_2.jpg",
                title: "这个世界上，或许没有真正的托黑"
            }, {
                img: "banner_3.jpg",
                title: "金童再见！西班牙前锋托雷斯宣布退役"
            }],
            newsList: [],
            dataSources: [
                {
                    title: "卡拉斯科：俱乐部一些人的态度令我不解；需要解决出现的问题",
                    source: "手机中国网",
                    cmtsNum: 2019,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_1.jpg"],
                    imgNum: 1
                }, {
                    title: "荷兰媒体：德利赫特接近加盟尤文，转会费7000万，年薪2000万",
                    source: "央视网新闻",
                    cmtsNum: 3620,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_2.jpg"],
                    imgNum: 1
                }, {
                    title: "申花客场1-0江苏终结九轮不胜，莫雷诺争议进球经VAR判罚有效",
                    source: "体坛大精汇",
                    cmtsNum: 5230,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: [],
                    imgNum: 0
                }, {
                    title: "卡拉斯科：俱乐部一些人的态度令我不解；需要解决出现的问题",
                    source: "体坛大精汇",
                    cmtsNum: 7690,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_3.jpg", "list_2.jpg", "list_1.jpg"],
                    imgNum: 20
                }, {
                    title: "敲锣鼓、放鞭炮！本周国际赛事MVP提名揭晓",
                    source: "体坛大精汇",
                    cmtsNum: 2019,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_3.jpg"],
                    imgNum: 1
                }, {
                    title: "申花客场1-0江苏终结九轮不胜，莫雷诺争议进球经VAR判罚有效",
                    source: "手机中国网",
                    cmtsNum: 2019,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_4.jpg"],
                    imgNum: 1
                }, {
                    title: "荷兰媒体：德利赫特接近加盟尤文，转会费7000万，年薪2000万",
                    source: "手机中国网",
                    cmtsNum: 2019,
                    isTop: true,
                    isVideo: true,
                    time: "00:58",
                    img: ["banner_2.jpg"],
                    imgNum: 1
                }, {
                    title: "敲锣鼓、放鞭炮！本周国际赛事MVP提名揭晓",
                    source: "体坛大精汇",
                    cmtsNum: 5230,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: [],
                    imgNum: 0
                }, {
                    title: "卡拉斯科：俱乐部一些人的态度令我不解；需要解决出现的问题",
                    source: "体坛大精汇",
                    cmtsNum: 7690,
                    isTop: true,
                    isVideo: false,
                    time: "00:00",
                    img: ["list_3.jpg", "list_2.jpg", "list_4.jpg"],
                    imgNum: 8
                }, {
                    title: "申花客场1-0江苏终结九轮不胜，莫雷诺争议进球经VAR判罚有效",
                    source: "手机中国网",
                    cmtsNum: 2019,
                    isTop: true,
                    isVideo: true,
                    time: "00:49",
                    img: ["banner_1.jpg"],
                    imgNum: 1
                }
            ],
            pageIndex: 1,
            loadding: true,
            pullUpOn: true,
            newsListStatus: false,
            animation: false,
            dataList: [
                {
                    name: "shop-fill",
                    title: '杂谈',
                    size: 30
                }, {
                    name: "order",
                    title: '同城',
                    size: 30
                }, {
                    name: "feedback",
                    title: '美文',
                    size: 30
                }, {
                    name: "like",
                    title: '生活',
                    size: 30
                }
            ],
            getType: [],
            getTypeIndex: null,
            getBulletin: [],
            getImage: [],
            left: 0,
            right: 40,
            bottom: 200,
            bgColor: "#5677fc",
            btnList: [],
            page: 1,
            totalPage: 1,
            key: "",
            hot: [],
        }
    },
    onLoad: function (options) {
        // 存储到访者来源
        if (options.path) {
            uni.setStorageSync('path', options.path);
        }

        this.pageIndex = 1;
        this.getForumListPage(this.pageIndex);
        // this.newsList = this.dataSources
        setTimeout(() => {
            this.animation = true
        }, 600)
        this.getNewsInfo();
    },
    onShareAppMessage(res) {
        // 分享带分享者id
        let title = '慧享优选社区'
        let path = '/pages/Ahx-extend/forum/forum'

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
    methods: {
        cleanKey: function () {
            this.key = ''
        },
        searchClick(page) {
            if (page == 1) {
                this.newsList = [];
                this.pullUpOn = true;
                this.newsListStatus = false;
            }
            uni.request({
                url: `${getApp().globalData.URL}/clientForum/searchInner?page=${page}`,
                method: 'POST',
                data: {
                    key: this.key
                },
                success: (res) => {
                    if (!res.data.data) {
                        this.newsListStatus = true;
                    }
                    this.newsList = res.data.data ? this.newsList.concat(res.data.data) : [];
                    this.totalPage = res.data.totalPage;
                    this.loadding = false
                }
            })
        },
        onClick(e) {
            let index = e.index

            let url = '../news-cmt/news-cmt-forum'
            if (this.getTypeIndex !== null) {
                url = '../news-cmt/news-cmt-forum?index=' + this.getType[this.getTypeIndex].id
            }

            console.log('--url->', url)

            uni.navigateTo({ url })
        },
        search: function () {
            uni.navigateTo({
                url: '../news-search/news-search'
            })
        },
        bannerDetail: function (id) {
            uni.navigateTo({
                url: `../newsDetail/newsDetail?id=${id}`
            })
        },
        detail(e) {
            let index = e.index;
            console.log('->this.', this.newsList[index])
            uni.navigateTo({
                url: `../newsDetail/newsDetail?id=${this.newsList[index].id}`
            })
        },
        selectType(id, index) {

            this.getTypeIndex = index;

            console.log('->e', id)
            this.loadding = true;
            this.pageIndex = 1;
            this.getForumListPage(this.pageIndex, id)
        },
        getNewsInfo() {
            uni.request({
                url: `${getApp().globalData.URL}/clientForum/getNewsInfo`,
                success: (res) => {
                    this.getImage = res.data.getImage ? res.data.getImage : []
                    this.getBulletin = res.data.getBulletin ? res.data.getBulletin : [{ title: '暂无公告' }]
                    this.getType = res.data.getType ? res.data.getType : []
                }
            })
        },
        getForumListPage(page, type) {
            if(this.key != '') {
                this.searchClick(page);
                return
            }
            if (page == 1) {
                this.newsList = [];
                this.pullUpOn = true;
                this.newsListStatus = false;
            }
            if (type) {
                page = page + '&type=' + type
            }
            uni.request({
                url: `${getApp().globalData.URL}/clientForum/getForumListPage?page=${page}`,
                success: (res) => {
                    if (!res.data.data) {
                        this.newsListStatus = true;
                    }
                    this.newsList = res.data.data ? this.newsList.concat(res.data.data) : [];
                    this.totalPage = res.data.totalPage;
                    this.loadding = false
                }
            })
        }
    },
    //页面相关事件处理函数--监听用户下拉动作
    onPullDownRefresh: function () {
        // this.newsList = this.dataSources;
        this.getTypeIndex = null;
        this.key = '';
        this.pageIndex = 1;
        this.pullUpOn = true;
        this.loadding = true;
        this.getForumListPage(this.pageIndex);
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
    onReachBottom: function () {
        if (!this.pullUpOn) return;
        this.loadding = true;
        if (this.pageIndex == this.totalPage) {
            this.loadding = false;
            this.pullUpOn = false
        } else {
            this.pageIndex = this.pageIndex + 1;
            this.getForumListPage(this.pageIndex);
        }
    },
}
</script>

<style>
page {
	background: #f2f2f2;
}


.tui-searchboxeasy {
	padding: 36rpx 20rpx 10rpx 20rpx;
	box-sizing: border-box;
	display: flex;
	align-items: center;
    background: #fff;
}

.tui-search-input {
	width: 100%;
	height: 66upx;
	border-radius: 35upx;
	padding: 0 30upx;
	box-sizing: border-box;
	background: #f2f2f2;
	display: flex;
	align-items: center;
	flex-wrap: nowrap;
}

.tui-input {
	flex: 1;
	color: #333;
	padding: 0 16upx;
	font-size: 28upx;
}

.tui-input-plholder {
	font-size: 28upx;
	color: #b2b2b2;
}

.tui-cancle {
	color: #888;
	font-size: 28upx;
	padding-left: 30upx;
	flex-shrink: 0;
}


.tui-container {
	display: flex;
	flex-direction: column;
	box-sizing: border-box;
	padding-bottom: env(safe-area-inset-bottom);
}

.tui-container .ul {
	width: calc(750rpx - 20rpx);
	min-height: 80rpx;
	height: auto;
	background: #fff;
	padding: 10rpx 10rpx 20rpx 10rpx;
	border-bottom: solid 1rpx #f9f9f9;
}

.tui-container .ul .li {
	float: left;
	width: calc(25% - 2px - 16rpx);
	height: calc(80rpx - 2px - 16rpx);
	line-height: calc(80rpx - 2px - 16rpx);
	color: rgb(40, 41, 44);
	font-size: 30rpx;
	font-weight: bold;
	text-align: center;
	border: solid 1px rgb(232, 232, 232);
	border-radius: 8rpx;
	margin: 8rpx;
	background: rgb(246, 247, 251);
}

.tui-container .ul .mor_active {
	background: #2dc195;
	color: #fff;
}

.tui-notice-board {
	width: 100%;
	padding-right: 30upx;
	box-sizing: border-box;
	font-size: 28upx;
	height: 60upx;
	background: #fff8d5;
	display: flex;
	align-items: center;
	/* position: fixed; */
	top: 0;
	/* #ifdef H5 */
	top: 44px;
	/* #endif */
	z-index: 999;
}

.tui-acatar {
	width: 30rpx;
	height: 30rpx;
	border-radius: 30rpx;
	display: block;
	margin-right: 16rpx;
}

.tui-icon-bg {
	background: #fff8d5;
	padding-left: 30upx;
	position: relative;
	z-index: 10;
}

.tui-icon-class {
	/* margin-right: 12upx; */
}

.tui-scorll-view {
	flex: 1;
	line-height: 1;
	white-space: nowrap;
	overflow: hidden;
	color: #f54f46;
}

.tui-notice {
	transform: translateX(100%);
}

.tui-animation {
	-webkit-animation: tui-rolling 12s linear infinite;
	animation: tui-rolling 12s linear infinite;
}

@-webkit-keyframes tui-rolling {
	0% {
		transform: translateX(100%);
	}

	100% {
		transform: translateX(-170%);
	}
}

@keyframes tui-rolling {
	0% {
		transform: translateX(100%);
	}

	100% {
		transform: translateX(-170%);
	}
}

.tui-grid-icon {
	width: 64upx;
	height: 64upx;
	margin: 0 auto;
	text-align: center;
	vertical-align: middle;
}

.tui-grid-label {
	display: block;
	text-align: center;
	font-weight: 400;
	color: #333;
	font-size: 28upx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
	margin-top: 10upx;
}

.tui-searchbox {
	padding: 16rpx 20rpx;
	box-sizing: border-box;
}

.tui-rolling-search {
	width: 100%;
	height: 60rpx;
	border-radius: 35rpx;
	padding: 0 40rpx 0 30rpx;
	box-sizing: border-box;
	background: #fff;
	display: flex;
	align-items: center;
	flex-wrap: nowrap;
	color: #999;
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

.tui-banner-swiper {
	width: 100%;
	height: 300rpx;
	position: relative;
}

.tui-slide-image {
	width: 100%;
	height: 300rpx;
	display: block;
}

.tui-banner-title {
	width: 100%;
	height: 100rpx;
	position: absolute;
	z-index: 9999;
	color: #fff;
	bottom: 0;
	padding: 0 30rpx;
	padding-top: 25rpx;
	font-size: 34rpx;
	font-weight: bold;
	background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.7));
	box-sizing: border-box;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

/* #ifdef APP-PLUS || MP */
.tui-banner-swiper .wx-swiper-dots.wx-swiper-dots-horizontal {
	width: 100%;
	top: 280rpx;
	text-align: right;
	padding-right: 30rpx;
	box-sizing: border-box;
}

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
	background: rgba(255, 255, 255, 0.9);
	border-radius: 8rpx;
}

.tui-banner-swiper .wx-swiper-dot-active::before {
	background: #5677fc;
}

.tui-banner-swiper .wx-swiper-dot.wx-swiper-dot-active {
	width: 18rpx;
}

/* #endif */

/* #ifdef H5 */
>>> .tui-banner-swiper .uni-swiper-dots.uni-swiper-dots-horizontal {
	width: 100%;
	top: 280rpx;
	text-align: right;
	padding-right: 30rpx;
	box-sizing: border-box;
}

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
	background: rgba(255, 255, 255, 0.9);
	border-radius: 8rpx;
}

>>> .tui-banner-swiper .uni-swiper-dot-active::before {
	background: #5677fc;
}

>>> .tui-banner-swiper .uni-swiper-dot.uni-swiper-dot-active {
	width: 18rpx;
}

/* #endif */

.tui-news-flex {
	width: 100%;
	display: flex;
}

.tui-flex-start {
	align-items: flex-start !important;
}

.tui-flex-center {
	align-items: center !important;
}

.tui-flex-column {
	flex-direction: column !important;
}

.tui-flex-between {
	justify-content: space-between !important;
}

.tui-news-cell {
	display: flex;
	padding: 20rpx 30rpx;
}

.tui-news-tbox {
	flex: 1;
	width: 100%;
	box-sizing: border-box;
	display: flex;
}

.tui-news-picbox {
	display: flex;
	position: relative;
}

.tui-w220 {
	width: 110rpx;
	margin-right: 20rpx;
}

.tui-h165 {
	height: 83rpx;
}

.tui-block {
	display: block;
}

.tui-w-full {
	width: 100%;
}

.tui-one-third {
	width: 33%;
}

.tui-news-title {
	width: 100%;
	font-size: 34rpx;
	word-break: break-all;
	word-wrap: break-word;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
	box-sizing: border-box;
}

.tui-pl-20 {
	padding-left: 20rpx;
}

.tui-pt20 {
	padding-top: 20rpx;
}

.tui-sub-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
	color: #999;
	box-sizing: border-box;
	line-height: 24rpx;
}

.tui-sub-source {
	font-size: 26rpx;
}

.tui-sub-cmt {
	font-size: 24rpx;
	line-height: 24rpx;
	display: flex;
	align-items: center;
}

.tui-tag {
	padding: 2rpx 6rpx !important;
	margin-left: 10rpx;
}

.tui-scale {
	transform: scale(0.6);
	transform-origin: center center;
}

.tui-btm-badge {
	position: absolute;
	right: 0;
	bottom: 0;
	font-size: 24rpx;
	color: #fff;
	padding: 2rpx 12rpx;
	background: rgba(0, 0, 0, 0.6);
	z-index: 20;
	transform: scale(0.8);
	transform-origin: 100% 100%;
}

.tui-video {
	position: absolute;
	z-index: 10;
	display: flex;
	align-items: center;
	justify-content: center;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
	transform-origin: 0 0;
}
/* 
    .tui-icon {
        background: rgba(0, 0, 0, 0.5);
        border-radius: 50%;
        padding: 26rpx;
    } */

.tui-icon-box .tui-icon {
	background: none;
	padding: 0;
	border-radius: 0;
}
</style>

