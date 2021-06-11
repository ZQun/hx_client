<template>
    <view class="tui-set-box">

        <uni-calendar 
            :insert="true"
            :lunar="true" 
            :showMonth="false"
            :hide="false"
            :start-date="date"
            :end-date="date"
            :selected="selected"
            @change="change"
        />

        <view class="tui-exit">
            <tui-button @tap="qian" type="green" height="88rpx">我要签到</tui-button>
        </view>
    </view>
</template>

<script>
import uniCalendar from '@/components/uni-calendar/uni-calendar'
import tuiListCell from "@/components/list-cell/list-cell"
import tuiButton from "@/components/extend/button/button"

export default {
    components: {
        uniCalendar,
        tuiListCell,
        tuiButton
    },
    data() {
        return {
            selected: [
                // {date: '2020-05-17', info: '签到'},
                // {date: '2020-05-18', info: '签到'},
                // {date: '2020-05-20', info: '签到'},
            ],
            date: ''
        }
    },
    onLoad() {
        this.getUserQian()
    },
    methods: {
        href(page) {
            let url = "";
            switch (page) {
                case 1:
                    url = "../userInfo/userInfo"
                    break;
                case 2:
                    url = "../address/address"
                    break;
                default:
                    break;
            }
            uni.navigateTo({
                url: url
            })
        },
        getUserQian() {
            uni.getStorage({
				key: 'token',
				success: (res) => {
					this.token = JSON.parse(res.data);
					uni.request({
						url: `${getApp().globalData.URL}/clientUser/getUserQian`,
						method: 'POST',
						header: {
							'Authorization': "Bearer " + this.token.token
						},
						success: (res) => {
                            this.selected = res.data.selected;
                            this.date = res.data.date;
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
        qian() {
            uni.getStorage({
				key: 'token',
				success: (res) => {
					this.token = JSON.parse(res.data);
					uni.request({
						url: `${getApp().globalData.URL}/clientUser/qian`,
						method: 'POST',
						header: {
							'Authorization': "Bearer " + this.token.token
						},
						success: (res) => {
                            this.getUserQian()
                            
							uni.showModal({
								title: res.data.title,
								content: res.data.content,
								showCancel: false,
								confirmText: '好的',
								success: function (res) {
									if (res.confirm) {
										console.log('用户点击确定');
									} else if (res.cancel) {
										console.log('用户点击取消');
									}
								}
							});
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
        change(e) {
            console.log(e);
        }
    }
}
</script>

<style>
.tui-set-box {
	padding-bottom: 20rpx;
	color: #333;
}

.tui-list-cell {
	display: flex;
	align-items: center;
	padding: 24rpx 30rpx;
	font-size: 30rpx;
}

.tui-info-box {
	font-size: 34rpx;
}

.tui-avatar {
	width: 140rpx;
	height: 140rpx;
	margin-right: 20rpx;
}

.tui-mtop {
	margin-top: 20rpx;
}

.tui-exit {
	padding: 100rpx 24rpx;
}
</style>
