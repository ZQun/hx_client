<template>
	<view class="container">
		<view class="tui-searchbox">
			<view class="tui-search-input">
				<!-- #ifdef APP-PLUS || MP -->
				<icon type="search" :size='13' color='#333'></icon>
				<!-- #endif -->
				<!-- #ifdef H5 -->
				<view>
					<tui-icon name="search" :size='16' color='#333'></tui-icon>
				</view>
				<!-- #endif -->
				<input confirm-type="search" :placeholder="hot.length != 0 ? '大家都在搜：' + hot[0] : '大家都在搜：慧享优选'" :focus="true" auto-focus placeholder-class="tui-input-plholder" class="tui-input" v-model.trim="key" @confirm="searchClick" />
				<!-- #ifdef APP-PLUS || MP -->
				<icon type="clear" :size='13' color='#bcbcbc' @tap="cleanKey" v-show="key"></icon>
				<!-- #endif -->
				<!-- #ifdef H5 -->
				<view @tap="cleanKey" v-show="key">
					<tui-icon name="close-fill" :size='16' color='#bcbcbc'></tui-icon>
				</view>
				<!-- #endif -->
			</view>
			<view class="tui-cancle" @tap="back">取消</view>
		</view>

		<view class="tui-search-history" v-if="history.length>0">
			<view class="tui-history-header">
				<view class="tui-search-title">搜索历史</view>
				<tui-icon name="delete" :size='14' color='#333' @tap="openActionSheet" class="tui-icon-delete"></tui-icon>
			</view>
			<view class="tui-history-content">
				<block v-for="(item,index) in history" :key="index">
					<tui-tag type="gray" shape="circle" @tap="onClickKey(index, 0)">{{item}}</tui-tag>
				</block>
			</view>
		</view>

		<view class="tui-search-hot">
			<view class="tui-hot-header">
				<view class="tui-search-title">大家正在搜</view>
			</view>
			<view class="tui-hot-content">
				<block v-for="(item,index) in hot" :key="index">
					<tui-tag type="gray" shape="circle" @tap="onClickKey(index, 1)">{{item}}</tui-tag>
				</block>
			</view>
		</view>
		<tui-actionsheet :show="showActionSheet" :tips="tips" @click="itemClick" @cancel="closeActionSheet"></tui-actionsheet>
	</view>

</template>

<script>
import tuiIcon from "@/components/icon/icon"
import tuiTag from "@/components/tag/tag"
import tuiActionsheet from "@/components/actionsheet/actionsheet"
export default {
	components: {
		tuiIcon,
		tuiTag,
		tuiActionsheet
	},
	data() {
		return {
			history: [],
			hot: [],
			key: "",
			showActionSheet: false,
			tips: "确认清空搜索历史吗？"
		}
	},
	onLoad() {
		this.getSearchListLog()
	},
	methods: {
		getSearchListLog() {
			uni.request({
				url: `${getApp().globalData.URL}/getSetting/getSearchListNew`,
				success: (res) => {
					if (res.data != '暂无数据') res.data.forEach((item, index) => {
						this.hot.push(item.title)
					})
				}
			})

			// 先存储个人搜索历史
			uni.getStorage({
				key: 'search',
				success: (res) => {
					this.history = JSON.parse(res.data);
				},
				fail: (fail) => { this.history = [] }
			});
		},
		searchClick() {
			let index = this.history.indexOf(this.key)
			// 先存储个人搜索历史
			if(index == -1) {
				if(this.history.length >= 10) {
					this.history.pop();
				}
				this.history.splice(0, 0, this.key);
			} else {
				this.history.splice(index, 1);
				this.history.splice(0, 0, this.key);
			}
			try {
				uni.setStorageSync('search', JSON.stringify(this.history));
			} catch (e) { }
			this.pathGo(this.key)
		},
		onClickKey(index, tap) {
			let s = {
				0: this.history[index],
				1: this.hot[index]
			}
			this.key = s[tap];
			this.searchClick();
		},
		pathGo(key) {
			uni.navigateTo({
				url: `../vip/productList?searchKey=${key}&type=search&state=vip`
			})
		},
		back: function () {
			uni.navigateBack();
		},
		cleanKey: function () {
			this.key = ''
		},
		closeActionSheet: function () {
			this.showActionSheet = false
		},
		openActionSheet: function () {
			this.showActionSheet = true
		},
		itemClick: function (e) {
			let index = e.index;
			if (index == 0) {
				this.showActionSheet = false;
				this.history = []
				try {
					uni.setStorageSync('search', JSON.stringify(this.history));
				} catch (e) { }
			}
		}
	}
}
</script>

<style>
page {
	color: #333;
	background: #fff;
}

.container {
	padding: 0 30upx 30upx 30upx;
	box-sizing: border-box;
}

.tui-searchbox {
	padding: 30upx 0;
	box-sizing: border-box;
	display: flex;
	align-items: center;
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

.tui-history-header {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 30upx 0;
}

.tui-icon-delete {
	padding: 10upx;
}

.tui-search-title {
	font-size: 28upx;
	font-weight: bold;
}

.tui-hot-header {
	padding: 30upx 0;
}

.tui-tag-class {
	display: inline-block;
	margin-bottom: 20upx;
	margin-right: 20upx;
	font-size: 26upx !important;
}
</style>
