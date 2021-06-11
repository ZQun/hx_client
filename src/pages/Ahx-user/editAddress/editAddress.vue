<template>
	<view class="tui-addr-box">
		<form :report-submit="true">
			<tui-list-cell :hover="false" padding="0">
				<view class="tui-line-cell">
					<view class="tui-title">收货人</view>
					<input v-model="addForm.name" placeholder-class="tui-phcolor" class="tui-input" name="name" placeholder="请输入收货人姓名" maxlength="15" type="text" />
				</view>
			</tui-list-cell>
			<tui-list-cell :hover="false" padding="0">
				<view class="tui-line-cell">
					<view class="tui-title">手机号码</view>
					<input v-model="addForm.phone" placeholder-class="tui-phcolor" class="tui-input" name="mobile" placeholder="请输入收货人手机号码" maxlength="11"
					type="number" />
				</view>
			</tui-list-cell>
			<tui-list-cell :arrow="true" padding="0">
				<view class="tui-line-cell">
					<view class="tui-title"><text class="tui-title-city-text">所在城市</text></view>
					<picker :value="value" mode="multiSelector" @change="picker" @columnchange="columnPicker" :range="multiArray">
						<!-- <button class="btn-primary" hover-class="btn-hover">请选择</button> -->
						<input placeholder-class="tui-phcolor" class="tui-input" disabled name="city" :placeholder="text" maxlength="50" type="text" />
					</picker>
					
				</view>
			</tui-list-cell>
			<tui-list-cell :hover="false" padding="0">
				<view class="tui-line-cell">
					<view class="tui-title">收货地址</view>
					<input v-model="addForm.address" placeholder-class="tui-phcolor" class="tui-input" name="address" placeholder="请输入详细的收货地址" maxlength="50" type="text" />
				</view>
			</tui-list-cell>
			<!-- <tui-list-cell :hover="false" padding="0">
				<view class="tui-line-cell">
					<view class="tui-cell-title">地址类型</view>
					<view class="tui-addr-label">
						<text v-for="(item,index) in lists" :key="index" class="tui-label-item" :class="{'tui-label-active':index==1}">{{item}}</text>
					</view>
				</view>
			</tui-list-cell> -->

			<!-- 默认地址 -->
			<tui-list-cell :hover="false" padding="0">
				<view class="tui-swipe-cell">
					<view>设为默认地址</view>
					<switch @change="switch2Change" :checked="addForm.status" color="#30CC67" class="tui-switch-small" />
				</view>
			</tui-list-cell>
			<!-- 保存地址 -->
			<view class="tui-addr-save">
				<tui-button @tap="addUserAddress(save)" type="green" height="88rpx">保存收货地址</tui-button>
			</view>
			<view class="tui-del" v-if="save !== 'addUserAddress'">
				<tui-button @tap="delUserAddress(addForm.id)" type="gray" height="88rpx">删除收货地址</tui-button>
			</view>
		</form>
	</view>
</template>

<script>
	import tuiButton from "@/components/extend/button/button"
	import tuiListView from "@/components/list-view/list-view"
	import tuiListCell from "@/components/list-cell/list-cell"
	export default {
		components: {
			tuiButton,
			tuiListView,
			tuiListCell
		},
		data() {
			return {
				lists: ["公司", "家", "学校", "其他"],
				selectList: [], //接口返回picker数据,此处就直接使用本地测试数据
				multiArray: [], //picker数据
				value: [0, 0, 0],
				text: "请选择城市",
				id: "",
				provice: [],
				selectListB: [],
				selectListC: [],
				addForm: {
					province: '',
					city: '',
					county: '',
					address: '',
					name: '',
					phone: '',
					status: false
				},
				save: 'addUserAddress',
			}
		},
		onLoad: function(options) {
			this.getChinazList('province');
			if(options.id !== undefined) {
				let id = Number(options.id);
				this.getUserAddressInfo(id);
				this.save = `updateUserAddress?id=${id}`
			}
		},
		methods: {
			addUserAddress(path) {
				// console.log('->this.addForm', this.addForm)
				if(this.addForm.name !== '' && this.addForm.phone !== '' && this.addForm.address !== '' && this.addForm.province !== '') {
					uni.getStorage({
						key: 'token',
						success: (res) => {
							this.token = JSON.parse(res.data);
							console.log('->res', this.token)
							
							uni.request({
								url: `${getApp().globalData.URL}/clientUser/${path}`,
								method: 'POST',
								header: {
									'Authorization': "Bearer " + this.token.token
								},
								data: this.addForm,
								success: (res) => {
									// this.userInfo = res.data !== '暂无数据' ? res.data : {};
									if(res.data.message == 'SUCCESS') {
										uni.showToast({ title: '保存成功！', duration: 1000 });
										setTimeout(() => { uni.navigateBack(); }, 1000);
									}
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
				} else {
					uni.showToast({
                        title: '请您补全信息！',
                        icon: 'none',
                        duration: 2000
                    });
				}
			},
			getUserAddressInfo(id) {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientUser/getUserAddressInfo?id=${id}`,
							method: 'GET',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							// data: this.addForm,
							success: (res) => {
								this.addForm = res.data;
								this.text = res.data.province +'-'+ res.data.city +'-'+ res.data.county;
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
			delUserAddress(id) {
				uni.getStorage({
					key: 'token',
					success: (res) => {
						this.token = JSON.parse(res.data);
						uni.request({
							url: `${getApp().globalData.URL}/clientUser/delUserAddress?id=${id}`,
							method: 'POST',
							header: {
								'Authorization': "Bearer " + this.token.token
							},
							// data: this.addForm,
							success: (res) => {
								if(res.data.message == 'SUCCESS') {
									uni.showToast({ title: '删除成功！', duration: 1000 });
									setTimeout(() => { uni.navigateBack(); }, 1000);
								}
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
			switch2Change(e) {
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
				this.addForm.status = e.target.value;
			},
			picker: function(e) {
				let value = e.detail.value;
				if (this.selectList.length > 0) {
					let provice = this.selectList[value[0]].name
					let city = this.selectListB[value[1]].name ? this.selectListB[value[1]].name : this.selectListB[value[1]];
					let district = this.selectListC[value[2]].name ? this.selectListC[value[2]].name : this.selectListC[value[2]];
					this.text = provice + "-" + city + "-" + district;
					// 整理表单
					this.addForm.province = provice;
					this.addForm.city = city;
					this.addForm.county = district;
				}
			},
			toArr(object) {
				let arr = [];
				for (let i in object) {
					arr.push(object[i].name);
				}
				return arr;
			},
			columnPicker: function(e) {
				//第几列 下标从0开始
				let column = e.detail.column;
				//第几行 下标从0开始
				let value = e.detail.value;
				if (column === 0) {
					// 获取城市列表
					this.getChinazList(`city&id=${this.selectList[value].province_id}`, column);
					this.value = [value, 0, 0]
				} else if (column === 1) {
					// 获取区县列表
					this.getChinazList(`country&id=${this.selectListB[value].city_id}`, column);
					this.value = [this.value[0], value, 0]
				}
			},
			getChinazList(type, column) {
				uni.request({
					url: `${getApp().globalData.URL}/clientUser/china?type=${type}`,
					success: (res) => {
						if(type === 'province') {
							this.selectList = res.data ? res.data : [];
							this.selectListB = this.toArr(res.data[0].city);
							this.selectListC = this.toArr(res.data[0].city[0].country)
							this.multiArray = [
								this.toArr(res.data),
								this.toArr(res.data[0].city),
								this.toArr(res.data[0].city[0].country)
							]
						}
						if(column === 0) {
							this.selectListB = res.data ? res.data : [];
							this.selectListC = this.toArr(res.data[0].city[0].country);
							this.multiArray.splice(1, 2, this.toArr(res.data), this.toArr(res.data[0].city[0].country))
						}
						if(column === 1) {
							this.selectListC = res.data ? res.data : [];
							this.multiArray.splice(2, 1, this.toArr(res.data))
						}
					}
				})
			}
		}
	}
</script>

<style>
	.container {
		padding: 100upx 30upx
	}

	.result {
		padding: 60upx 20upx;
		font-size: 30upx;
		color: #333
	}

	.btn-gray,
	.btn-disabled {
		margin-bottom: 32upx
	}
	.tui-addr-box {
		padding: 20rpx 0;
	}

	.tui-line-cell {
		width: 100%;
		padding: 24rpx 30rpx;
		box-sizing: border-box;
		display: flex;
		align-items: center;
	}

	.tui-title {
		width: 180rpx;
		font-size: 28rpx;
	}

	.tui-title-city-text {
		width: 180rpx;
		height: 40rpx;
		display: block;
		line-height: 46rpx;
	}

	.tui-input {
		width: 500rpx;
	}

	.tui-input-city {
		flex: 1;
		height: 40rpx;
		font-size: 28rpx;
		padding-right: 30rpx;
	}

	.tui-phcolor {
		color: #ccc;
		font-size: 28rpx;
	}
	.tui-cell-title{
		font-size: 28rpx;
	}
	.tui-addr-label {
		margin-left: 70rpx;
	}

	.tui-label-item {
		width: 76rpx;
		height: 40rpx;
		border: 1rpx solid rgb(136, 136, 136);
		border-radius: 6rpx;
		font-size: 26rpx;
		text-align: center;
		line-height: 40rpx;
		margin-right: 20rpx;
		display: inline-block;
		transform: scale(0.9);
	}
	.tui-label-active{
		background: #E41F19;
		border-color:#E41F19;
		color: #fff;
	}
	.tui-swipe-cell {
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: #fff;
		padding: 10rpx 24rpx;
		border-radius: 6rpx;
		overflow: hidden;
		font-size: 28rpx;
	}

	.tui-switch-small {
		transform: scale(0.8);
		transform-origin: 100% center;
	}

	/* #ifndef H5 */
	.tui-switch-small .wx-switch-input {
		margin: 0 !important;
	}

	/* #endif */

	/* #ifdef H5 */
	>>>uni-switch .uni-switch-input {
		margin-right: 0 !important;
	}

	/* #endif */

	.tui-addr-save {
		padding: 24rpx;
		margin-top: 100rpx;
	}

	.tui-del {
		padding: 24rpx;
	}
</style>
