<template>
	<view class="container">
		<button class="btn-primary btn-gray" hover-class="btn-hover">禁用状态</button>
		<button class="btn-primary btn-disabled" hover-class="btn-hover">禁用状态</button>
		<picker :value="value" mode="multiSelector" @change="picker" @columnchange="columnPicker" :range="multiArray">
			<button class="btn-primary" hover-class="btn-hover">请选择</button>
		</picker>
		<view class="result">
			{{text}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectList: [], //接口返回picker数据,此处就直接使用本地测试数据
				multiArray: [], //picker数据
				value: [0, 0, 0],
				text: "",
				id: "",
				provice: [],
				selectListB: [],
				selectListC: []
			}
		},
		onLoad: function() {
			this.getChinazList('province');
		},
		methods: {
			picker: function(e) {
				let value = e.detail.value;
				if (this.selectList.length > 0) {
					let provice = this.selectList[value[0]].name
					let city = this.selectListB[value[1]].name ? this.selectListB[value[1]].name : this.selectListB[value[1]];
					let district = this.selectListC[value[2]].name ? this.selectListC[value[2]].name : this.selectListC[value[2]];
					this.text = provice + "-" + city + "-" + district;
					this.address = { provice, city, country: district };
					console.log('->text', this.address);
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
</style>
