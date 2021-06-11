<template>
	<view class="container">
		<view class="tui-news-title">
			{{ news.title }}
		</view>
		<view class="tui-sub-info">
			<view class="tui-sub-left">
				<text class="tui-author">{{ news.hx_user.name }}</text>
				<text>{{ news.created_at }}</text>
			</view>
			<view class="tui-sub-right">阅读 {{ news.read_num }}</view>
		</view>
		<view class="tui-news-content">
			<view class="tui-article">
				<u-parse :content="news.content_text" @preview="preview" @navigate="navigate" />
			</view>

			<image v-for="(item, index) in news.img_url" :key="index" :src="item" class="tui-article-pic" mode="widthFix"></image>
		</view>

		<!-- <view class="tui-news-source">消息参考来源：体坛大精汇</view> -->

		<!-- <view class="tui-operate-box">
			<tui-tag :type="isFabulous?'':'gray'" shape="circle" :plain="true" @click="btnFabulous">
				<tui-icon :name="iconName(isFabulous)" :size="20" :color="iconColor"></tui-icon>
				<text class="tui-black" :class="[isFabulous?'tui-primary':'']">{{fabulous}}赞</text>
			</tui-tag>
			<button open-type="share" class="tui-share-btn">
				<tui-tag type="gray" shape="circle" :plain="true">
					<tui-icon name="partake" :size="20" color="#333"></tui-icon>
					<text class="tui-black">分享</text>
				</tui-tag>
			</button>

		</view> -->

		<view class="tui-cmt-title">评论（{{ news.comment_num }}）</view>
		<view class="tui-cmtbox">
			<view class="tui-cmt-cell" v-for="(item, index) in commentList" :key="index">
				<image :src="item.hx_user.img_url" class="tui-avatar"></image>
				<view class="tui-cmt-detail">
					<view class="tui-header-box">
						<view class="tui-cmt-nickname">{{ item.hx_user.name }}</view>
						<view class="tui-fabulous" :class="[item.isFabulous ? 'tui-primary' : '']" :id="index" @tap="cmtFabulous(item.id)">
							<text>{{ item.like_size == 0 ? '赞' : item.like_size }}</text>
							<tui-icon :name="iconName(item.isFabulous)" :size="15" :color="itemIconColor(item.isFabulous)"></tui-icon>
						</view>
					</view>
					<view class="tui-cmt-content">
						{{ item.content_text }}
					</view>
					<!-- <view class="tui-reply-box" v-if="item.replayNum>0">
						<tui-list-cell bgcolor="#f2f2f2" :size="28" v-for="(items,index2) in item.reply" :key="index2" :last="item.replayNum<2 && item.reply.length-1==index"
						@tap="cmtReply">
							<view class="tui-flex-1 tui-reply-nickname">{{items.nickname}}</view>
							<view class="tui-flex-1">{{items.content}}</view>
						</tui-list-cell>
						<tui-list-cell bgcolor="#f2f2f2" :size="28" :last="true" v-if="item.replayNum>2" @tap="cmtReply">
							<view class="tui-flex-1  tui-cell-last">
								<text>共{{item.replayNum}}条回复</text>
								<tui-icon name="arrowright" :size="22" color="#5677fc"></tui-icon>
							</view>
						</tui-list-cell>
					</view> -->
					<view class="tui-footer">
						{{ item.created_at }}
						<!-- <view class="tui-primary tui-ml" hover-class="opcity" :hover-start-time="150" @tap="cmtReply">回复</view> -->
					</view>
				</view>
			</view>
		</view>

		<view class="tui-operation">
			<view class="tui-operation-left tui-col-7 tui-height-full tui-ptop-zero">
				<view class="tui-btn-comment" @tap="btnCmt">发表你的评论...</view>
			</view>
			<view class="tui-operation-right tui-right-flex tui-col-5">
				<view class="tui-operation-item" hover-class="opcity" :hover-stay-time="150" @tap="btnCmt">
					<tui-icon name="message" :size="30" color="#444"></tui-icon>
					<!-- <tui-badge type="white" size="small">501</tui-badge> -->
				</view>
				<!-- <view class="tui-operation-item" @tap="collection">
					<tui-icon :name="isCollection?'star-fill':'star'" :size="29" :color="isCollection?'#5677fc':'#444'"></tui-icon>
				</view>
				<view class="tui-operation-item" hover-class="opcity" :hover-stay-time="150">
					<button open-type="share" class="tui-share-btn">
						<tui-icon name="share" :size="31" color='#444'></tui-icon>
					</button>
				</view> -->
			</view>
		</view>

		<!--加载loadding-->
		<tui-loadmore :visible="loadding" :index="3" type="primary"></tui-loadmore>
		<tui-nomore :visible="!pullUpOn" bgcolor="#fff" text="没有更多评论"></tui-nomore>
		<!--加载loadding-->
		<view class="tui-safearea-bottom"></view>
	</view>
</template>

<script>
import tuiIcon from '@/components/icon/icon';
import tuiTag from '@/components/tag/tag';
import tuiListCell from '@/components/list-cell/list-cell';
import tuiLoadmore from '@/components/loadmore/loadmore';
import tuiNomore from '@/components/nomore/nomore';
import tuiBadge from '@/components/badge/badge';
import uParse from '@/components/gaoyia-parse/parse.vue'
import { Base64 } from 'js-base64';

export default {
	components: {
		tuiIcon,
		tuiTag,
		tuiListCell,
		tuiLoadmore,
		tuiNomore,
		tuiBadge,
		uParse
	},
	data() {
		return {
			fabulous: 123,
			isFabulous: false,
			isCollection: false,
			cmtList: [
				{
					avatar: 'list_2.jpg',
					nickname: '米兰的卡先生',
					fabulous: 123,
					isFabulous: false,
					content: '我一直没懂赛前问一个主教练如何评价对手的主教练， 记者究竟是想得到什么答案？',
					reply: [
						{
							nickname: 'Mesaldo',
							content: '汉军威武!卓尔不凡!火炉德比，热力四射！场上争胜，场下朋友☺ ☻',
						},
						{
							nickname: '月牙',
							content: '新政实行后，大小摩托轮流冲，不用经常上迪力了，杨帅该拿下就拿下☺',
						},
					],
					replayNum: 44,
					time: '昨天 22:12',
				},
				{
					avatar: 'avatar_1.jpg',
					nickname: '月牙',
					fabulous: 2,
					content: '力帆有杨帅，迪力木来提，尹聪耀，完全可以应付。尤其是杨帅坐稳主力后卫。',
					reply: [
						{
							nickname: 'thorui',
							content: '汉军威武!卓尔不凡!火炉德比，热力四射！场上争胜，场下朋友',
						},
						{
							nickname: 'Mr卡卜斯',
							content: '说实话，武汉重庆还真的是一家，但是比赛还是要分出个胜负来的，我卓尔球迷肯定是要为我武汉加油了，按照目前两队现在这个状态来看武汉重庆应该是五五开，所以我想说武汉加油！卓尔加油！',
						},
					],
					replayNum: 2,
					time: '昨天 21:09',
				},
				{
					avatar: 'avatar_2.jpg',
					nickname: 'thorui',
					fabulous: 0,
					content: '小克鲁伊夫带的球队征服了中超球迷，李铁也带队冲超成功，现在风头正劲，究竟鹿死谁手，谁更胜一筹，期待精彩的比赛 ☻ ☻ ☻',
					reply: [],
					replayNum: 0,
					time: '昨天 17:30',
				},
			],
			pageIndex: 1,
			loadding: false,
			pullUpOn: true,
			id: 0,
			news: {
				title: '',
				created_at: '',
				read_num: 0,
				content_text: `<div></div>`,
				img_url: [],
				hx_user: {
					name: '',
				},
				comment_num: 0,
			},
			totalPage: 1,
			commentList: [],
		};
	},
	onLoad(options) {
		// 存储到访者来源
		if (options.path) {
			uni.setStorageSync('path', options.path);
		}

		console.log('---->options', options);
		this.id = Number(options.id);
		this.getForumID(this.id);
	},
	onShareAppMessage(res) {
		// 分享带分享者id
		let title = this.searchKey;
		let path = '/pages/Ahx-extend/newsDetail/newsDetail';

		try {
			const value = uni.getStorageSync('token');
			if (value) {
				let id = JSON.parse(value).id;
				return { title, path: `${path}?path=${Base64.encode(id)}&id=${this.id}` };
			} else {
				return { title, path: `${path}?id=${this.id}` };
			}
		} catch (e) {
			return { title, path: `${path}?id=${this.id}` };
		}
	},
	onShow() {
		if (this.news) {
			this.getForumID(this.id);
		}
	},
	computed: {
		iconColor() {
			return this.isFabulous ? '#5677fc' : '#333';
		},
		itemIconColor() {
			return function(isFabulous) {
				return isFabulous ? '#5677fc' : '#9a9a9a';
			};
		},
		iconName() {
			return function(isFabulous) {
				return isFabulous ? 'agree-fill' : 'agree';
			};
		},
	},
	methods: {
		preview(src, e) {
			// do something
		},
		navigate(href, e) {
			// do something
		},
		btnFabulous: function() {
			this.fabulous = this.isFabulous ? 123 : 124;
			this.isFabulous = !this.isFabulous;
		},
		cmtFabulous: function(id) {
			console.log(id);
			// 需要登录
			uni.getUserInfo({
				lang: 'zh_CN',
				success: (res) => {
					// 判断登录状态否
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							uni.request({
								url: `${getApp().globalData.URL}/clientForum/submitLike`,
								method: 'POST',
								header: {
									Authorization: 'Bearer ' + this.token.token,
								},
								data: {
									id,
								},
								success: (res) => {
									if (res.data.message === 'SUCCESS') {
										uni.showToast({ title: '已点赞', duration: 1000 });
										this.pageIndex = 1;
										this.getForumID(this.id, this.pageIndex);
									}
									if (res.data == 'RATE_LIMIT_EXCEEDED') {
										uni.showToast({ title: '您点的太快了', duration: 1000 });
									}
								},
							});
						},
						fail: (fail) => {
							uni.navigateTo({
								url: '/pages/Ahuixiang/login/login',
								animationType: 'pop-in',
								animationDuration: 200,
							});
						},
					});
				},
				fail: (res) => {
					uni.navigateTo({
						url: '/pages/Ahuixiang/login/login',
						animationType: 'pop-in',
						animationDuration: 200,
					});
				},
			});
		},
		collection: function() {
			this.isCollection = !this.isCollection;
			if (this.isCollection) {
				this.tui.toast('收藏成功！');
			}
		},
		btnCmt: function() {
			uni.navigateTo({
				url: `../news-cmt/news-cmt?id=${this.id}`,
			});
		},
		cmtAll: function() {
			uni.navigateTo({
				url: '../news-cmt-list/news-cmt-list',
			});
		},
		cmtReply: function() {
			uni.navigateTo({
				url: '../news-cmt-reply/news-cmt-reply',
			});
		},
		getForumID(id, page) {
			if (page) {
				id = id + '&page=' + page;
			}
			uni.request({
				url: `${getApp().globalData.URL}/clientForum/getForumID?id=${id}`,
				success: (res) => {
					this.news = res.data;
					this.news.img_url = JSON.parse(this.news.img_url);
					if (res.data.comment) {
						if (page == 1 || page == null || page == undefined) {
							this.commentList = [];
							this.pullUpOn = true;
						}
						this.commentList = res.data.comment.data ? this.commentList.concat(res.data.comment.data) : [];
						this.totalPage = res.data.comment.totalPage;
						this.loadding = false;
					}
				},
			});
		},
	},
	// 页面上拉触底事件的处理函数
	onReachBottom: function() {
		if (!this.pullUpOn) return;
		this.loadding = true;
		if (this.pageIndex == this.totalPage) {
			this.loadding = false;
			this.pullUpOn = false;
		} else {
			this.pageIndex = this.pageIndex + 1;
			this.getForumID(this.id, this.pageIndex);
		}
	},
};
</script>

<style>
page {
	background: #fff;
	color: #333;
}

.container {
	padding: 40rpx 30rpx 110rpx 30rpx;
	box-sizing: border-box;
}

.tui-news-title {
	font-size: 48rpx;
	font-weight: 500;
	text-align: justify;
}

.tui-sub-info {
	padding-top: 30rpx;
	display: flex;
	align-items: center;
	justify-content: space-between;
	font-size: 28rpx;
	color: #999;
}

.tui-author {
	color: #5677fc;
	padding-right: 20rpx;
}

.tui-news-content {
	padding-top: 40rpx;
}

.tui-article {
	/* text-indent: 2em; */
	font-size: 34rpx;
	padding-bottom: 40rpx;
	line-height: 60rpx;
	text-align: justify;
	word-break: break-all;
	word-wrap: break-word;
}

.tui-article-pic {
	width: 100%;
	display: block;
	margin-bottom: 40rpx;
}

.tui-news-source {
	font-size: 24rpx;
	color: #999;
}

.tui-operate-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 60rpx 40rpx;
	box-sizing: border-box;
}

.tui-tag-class {
	min-width: 130rpx;
	padding: 20rpx 52rpx !important;
	font-size: 26rpx !important;
	display: flex;
	align-items: center;
	justify-content: center;
}

.tui-black {
	color: #333;
	padding-left: 12rpx;
}

.tui-cmt-title {
	font-size: 30rpx;
	font-weight: bold;
	position: relative;
}

.tui-cmt-title::after {
	content: '';
	position: absolute;
	left: -18rpx;
	top: 18%;
	width: 6rpx;
	height: 64%;
	background: #5677fc;
}

.tui-cmtbox {
	padding-bottom: 20rpx;
}

.tui-cmt-cell {
	display: flex;
	align-items: flex-start;
	justify-content: space-between;
	padding-top: 44rpx;
}

.tui-avatar {
	width: 64rpx;
	height: 64rpx;
	border-radius: 32rpx;
	display: block;
	flex-shrink: 0;
}

.tui-cmt-detail {
	width: 100%;
	padding-left: 16rpx;
	box-sizing: border-box;
}

.tui-header-box {
	display: flex;
	align-items: flex-start;
	justify-content: space-between;
	font-size: 30rpx;
}

.tui-cmt-nickname {
	color: #5677fc;
}

.tui-fabulous {
	color: #9a9a9a;
}

.tui-fabulous text {
	padding-right: 4rpx;
	font-size: 24rpx;
}

.tui-cmt-content {
	font-size: 32rpx;
	color: #333;
	text-align: justify;
	padding-top: 8rpx;
	word-break: break-all;
	word-wrap: break-word;
}

.tui-reply-box {
	border-radius: 8rpx;
	overflow: hidden;
	margin-top: 16rpx;
}

.tui-cell-class {
	flex-direction: column;
	justify-content: flex-start !important;
	padding: 20rpx !important;
	text-align: justify;
	word-break: break-all;
	word-wrap: break-word;
}

.tui-cell-last {
	color: #5677fc;
}

.tui-flex-1 {
	flex: 1;
	width: 100%;
}

.tui-reply-nickname {
	font-size: 24rpx;
	color: #7a7a7a;
	padding-bottom: 8rpx;
}

.tui-footer {
	display: flex;
	align-items: center;
	font-size: 24rpx;
	margin-top: 16rpx;
	color: #9a9a9a;
}

.tui-primary {
	color: #5677fc !important;
}

.tui-ml {
	margin-left: 16rpx;
}

.tui-cell-last .tui-icon-class {
	width: 40rpx !important;
	margin-left: -10rpx;
}

.tui-operation {
	width: 100%;
	height: 100rpx;
	overflow: hidden;
	background: #fff;
	display: flex;
	align-items: center;
	justify-content: space-between;
	position: fixed;
	left: 0;
	bottom: 0;
	z-index: 99999;
	padding-bottom: env(safe-area-inset-bottom);
}

.tui-safearea-bottom {
	width: 100%;
	height: env(safe-area-inset-bottom);
}

.tui-operation::before {
	content: '';
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	border-top: 1rpx solid #eaeef1;
	-webkit-transform: scaleY(0.5);
	transform: scaleY(0.5);
}

.tui-operation-left {
	display: flex;
	align-items: center;
}

.tui-operation-item {
	flex: 1;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	position: relative;
}

.tui-operation-right {
	height: 100rpx;
	box-sizing: border-box;
	padding-top: 0;
}

.tui-badge-class {
	color: #5677fc !important;
	position: absolute;
	top: -6rpx;
	padding: 2px 4px !important;
	/* #ifdef H5 */
	transform: translateX(50%) scale(0.8);
	/* #endif */
}

.tui-right-flex {
	display: flex;
	align-items: center;
	justify-content: center;
}

.tui-btn-comment {
	height: 64rpx;
	width: 96%;
	background: #ededed;
	color: #999;
	border-radius: 8rpx;
	font-size: 28rpx;
	display: flex;
	align-items: center;
	padding-left: 20rpx;
	box-sizing: border-box;
	padding-top: 0;
	margin-left: 30rpx;
}

.tui-col-7 {
	width: 88.33333333%;
}

.tui-col-5 {
	width: 11.66666667%;
}

.tui-share-btn {
	display: block;
	background: none;
	margin: 0;
	padding: 0;
}
</style>
