<template>
	<!--tabbar-->
    <view class="tui-tabbar">
        <block v-for="(item,index) in tabbar" :key="index">
            <view class="tui-tabbar-item" :class="[current==index?'tui-item-active':'']" :data-index="index" @tap="tabbarSwitch">
                <view :class="[index==0?'tui-ptop-4':'']">
                    <tui-icon :name="current==index?item.icon+'-fill':item.icon" :color="current==index?'#2DC195':'#666'" :size="item.size"></tui-icon>
                </view>
                <view class="tui-scale">{{item.text}}</view>
            </view>
        </block>
    </view>
    <!--tabbar-->
</template>

<script>
    import tuiIcon from "@/components/icon/icon"
	
	export default {
        name: 'Menu',
		components: {
			tuiIcon
		},
		data() {
			return {
                // current: 0,
				tabbar: [
					{
						icon: "home",
						text: "首页",
						size: 21
					},
					{
						icon: "clock",
						text: "活动",
						size: 21
					},
					{
						icon: "like",
						text: "VIP",
						size: 21
					},
					{
						icon: "message",
						text: "论坛",
						size: 22
					},
					{
						icon: "people",
						text: "我的",
						size: 24
					}
                ],
            }
        },
        props: {
            current: {
                type: Number,
                default: 0
            }
        },
		methods: {
            tabbarSwitch: function(e) {
				let index = e.currentTarget.dataset.index;
                // this.current = index;

                if(this.current !== index) {
                    switch(this.tabbar[index].text) {
                        case '首页': {
                            uni.redirectTo({ url: '/pages/Ahuixiang/index/index' })
                            break
                        }
                        case '活动': {
                            uni.redirectTo({ url: '/pages/Ahx-extend/exercise/exercise' })
                            break
                        }
                        case 'VIP': {
                            uni.redirectTo({ url: '/pages/Ahx-extend/vip/vip' })
                            break
                        }
                        case '论坛': {
                            uni.redirectTo({ url: '/pages/Ahx-extend/forum/forum' })
                            break
                        }
                        case '我的': {
                            uni.redirectTo({ url: '/pages/Ahuixiang/my/my' })
                            break
                        }
                    }
                }
			},
        },
	}
</script>

<style>
	/*tabbar*/
	.tui-tabbar {
		width: 100%;
		position: fixed;
		display: flex;
		align-items: center;
		justify-content: space-between;
		z-index: 99999;
		background: #fff;
		height: 100rpx;
		left: 0;
		bottom: 0;
		padding-bottom: env(safe-area-inset-bottom);
	}

	.tui-safearea-bottom {
		width: 100%;
		height: env(safe-area-inset-bottom);
	}

	.tui-tabbar::before {
		content: '';
		width: 100%;
		border-top: 1rpx solid #d9d9d9;
		position: absolute;
		top: 0;
		left: 0;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
	}

	.tui-tabbar-item {
		flex: 1;
		width: 25%;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: space-between;
		font-size: 24rpx;
		color: #666;
		height: 80rpx;
	}

	.tui-ptop-4 {
		padding-top: 4rpx;
	}

	.tui-scale {
		font-weight: bold;
		transform: scale(0.8);
		transform-origin: center 100%;
		line-height: 30rpx;
	}

	.tui-item-active {
		color: #2DC195 !important;
	}
	/*tabbar*/
</style>
