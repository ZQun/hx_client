<template>
	<view class="content">
		<image class="logo" :src="src"></image>
		<view class="name">
			<text class="title">{{title}}</text>
		</view>
        <button class="but" v-show="butState" :loading="loading" :disabled="disabled" type="primary" open-type="getUserInfo" @getuserinfo="onClickZan">登录</button>
        <button class="but" v-show="!butState" @click="onNext">返回上一页</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
                title: '请您登录后操作！',
                src: '../../../static/images/mall/user.jpg',
                loading: false,
                disabled: false,
                butState: true,
			}
		},
		onLoad() {

		},
		methods: {
            onClickZan(e) {
                console.log('->e login', e)

                if(e.target.errMsg != 'getUserInfo:fail auth deny') {
                    this.loading = true;
                    this.disabled = true;
                    this.src = e.target.userInfo.avatarUrl;
                    this.title = e.target.userInfo.nickName;

                    // 存储用户信息
                    try {
                        uni.setStorageSync('avatarUrl', this.src);
                        uni.setStorageSync('nickName', this.title);
                    } catch (e) {
                        // error
                    }

                    uni.login({
                        success: (res) => {
                            console.log('login:', res);
                            uni.request({
                                url: `${getApp().globalData.URL}/clientUser/login?code=${res.code}&img=${this.src}&name=${this.title}`,
                                success: (response) => {
                                    console.log('response', response);
                                    this.butState = false;

                                    uni.setStorage({
                                        key: 'token',
                                        data: JSON.stringify(response.data),
                                        success: () => {
                                            console.log('success');
                                            this.onNext();
                                        }
                                    });
                                }
                            });
                        },
                        fail: (res) => {
                            console.log('->fail', res)
                        }
                    });
                } else {
                    uni.showToast({
                        title: '请您登录后操作！',
                        icon: 'none',
                        duration: 2000
                    });
                }
            },
            onNext() {
                console.log('-return')
                uni.navigateBack();
            }
		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
    }
    
    .name {
        margin: 10px 0 50px 0;
    }

	.title {
		font-size: 36upx;
        color: #8f8f94;
    }
    
    .but {
        width: 300rpx;
        margin: 0 auto;
        text-align: center;
    }
</style>
