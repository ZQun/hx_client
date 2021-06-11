<template>
	<view class="container">
    <tui-list-cell style="margin: 10rpx 0;" :hover="false">
      <view class="tui-line-cell">
        <view class="tui-title">{{old.title}}</view>
        <input v-model="title" placeholder-class="phcolor" class="tui-input" name="email" :placeholder="old.titlePlac" maxlength="50" type="text" />
      </view>
    </tui-list-cell>
		<view class="tui-cells">
        <textarea v-model="text" class="tui-textarea" name="desc" :placeholder="old.innerPlac" maxlength="500" placeholder-class="tui-phcolor-color"
        auto-focus />
        <view class="tui-textarea-counter">{{sizeNum}}</view>
    </view>
    <tui-list-cell style="margin: 10rpx 0;" :hover="false">
      <view class="tui-line-cell">
        <view class="tui-title">{{old.typeTitle}}</view>
        <picker @change="bindPickerChange" :value="index" :range="array" class="tui-input-select">
            <view class="uni-input">{{array[index]}}</view>
        </picker>
      </view>
    </tui-list-cell>
    <view class="tui-enclosure">
      <tui-upload :serverUrl="uploadURL"  @complete="result" @remove="remove"></tui-upload>
    </view>
    <!-- <view class="tui-enclosure">
      <tui-icon name="satisfied" size="25" class="tui-mr"></tui-icon>
      <tui-icon name="picture" size="25" class="tui-mr"></tui-icon>
      <tui-icon name="link" size="22" class="tui-mr"></tui-icon>
    </view> -->
    <view class="tui-cmt-btn">
      <tui-button @tap="submitForum">确认发布</tui-button>
    </view>
    <!--toast提示-->
		<tui-toast ref="toast"></tui-toast>
  </view>
</template>

<script>
	import tuiIcon from "@/components/icon/icon"
  import tuiButton from "@/components/button/button"
  import tuiListCell from "@/components/list-cell/list-cell"
  import tuiUpload from '@/components/tui-upload/tui-upload'
  import tuiToast from "@/components/extend/toast/toast"
	export default {
		components: {
			tuiIcon,
      tuiButton,
      tuiListCell,
      tuiUpload,
      tuiToast,
      tuiListCell
		},
		data() {
			return {
        array: [],
        index: 0,
        imageData: [],
        old: {
          title: '帖子标题',
          titlePlac: '请输入帖子标题',
          innerPlac: '发表你的帖子内容...',
          typeTitle: '帖子分类'
        },
        text: '',
        title: '',
        type: 0,
        getTypeIndex: null
			}
    },
    computed: {
      uploadURL: function() {
        return getApp().globalData.URL + "/clientForum/upload"
      },
      sizeNum: function() {
        let len = this.text.length
        if(len > 500) return '超限/500'
        return len + '/500'
      }
    },
    onLoad(options) {

      console.log('->options', options.index)
      if(options.index) {
        this.getTypeIndex = Number(options.index)
      }

      this.getForumType()
    },
		methods: {
      submitForum() {
        // 整理数据
        let type_id = this.typeAll[this.index].id
        let data = {
          title: this.title, // 帖子标题
          content_text: this.text, // 帖子内容
          type_id,// 帖子所属分类id
          img_url: this.imageData,// 帖子上传图片
        }
        if(data.title == '' || data.content_text == '') {
          this.tui.toast('请补全信息！')
        } else {
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
                    url: `${getApp().globalData.URL}/clientForum/addForumInfo`,
                    method: 'POST',
                    header: {
                      'Authorization': "Bearer " + this.token.token
                    },
                    data,
                    success: (res) => {
                      if(res.data.message === 'SUCCESS') {
                        uni.showToast({ title: '发布成功！', duration: 1000 });
                        setTimeout(() => {
                          uni.navigateBack();
                        }, 1000)
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
            fail: (res) => {
              uni.navigateTo({
                url: '/pages/Ahuixiang/login/login',
                animationType: 'pop-in',
                animationDuration: 200
              });
            }
          })
        }
        console.log('--->data', data)
      },
      getForumType() {
        uni.request({
					url: `${getApp().globalData.URL}/clientForum/getForumTypeAll`,
					success: (res) => {
            if(res.data !== '暂无数据') {
              this.typeAll = res.data;
              res.data.forEach((item, index) => {
                if(this.getTypeIndex !== null) {
                  if(item.id == this.getTypeIndex) {
                    this.index = index;
                  }
                }
                this.array.push(item.title)
              })
              // this.index = this.getTypeIndex;
            }
					}
				})
      },
			bindPickerChange: function(e) {
          console.log('picker发送选择改变，携带值为', e.target.value)
          this.index = e.target.value
      },
      result: function(e) {
				console.log('e', e)
				this.imageData = e.imgArr;
			},
			remove: function(e) {
				//移除图片
				console.log(e)
				let index = e.index
			}
		}
	}
</script>

<style>
page {
  background: #fff;
  color: #333;
}
.container{
  padding: 30upx;
  box-sizing: border-box;
}

.tui-line-cell {
  width: 100%;
  box-sizing: border-box;
  display: flex;
  align-items: center;
}

.tui-title {
  line-height: 32rpx;
  flex-shrink: 0;
}

.tui-input {
  font-size: 32rpx;
  color: #333;
  padding-left: 20rpx;
  flex: 1;
}

.tui-input-select {
  font-size: 32rpx;
  color: #333;
  padding-left: 20rpx;
  flex: 1;
  border: solid 1px #2DC195;
  margin-left: 20rpx;
  padding: 5rpx 20rpx;
  font-weight: bold;
  color: #000;
  border-radius: 10rpx;
  width: 200rpx;
}

.tui-cells {
  border-radius: 4upx;
  height: 280upx;
  box-sizing: border-box;
  padding: 20upx 20upx 0 20upx;
  position: relative;
}

.tui-cells::after {
  content: '';
  position: absolute;
  height: 200%;
  width: 200%;
  border: 1px solid #e6e6e6;
  transform-origin: 0 0;
  -webkit-transform-origin: 0 0;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
  left: 0;
  top: 0;
  border-radius: 8upx;
}

.tui-textarea {
  height: 210upx;
  width: 100%;
  color: #666;
  font-size: 28upx;
}

.tui-phcolor-color {
  color: #ccc !important;
}

.tui-textarea-counter {
  font-size: 24upx;
  color: #999;
  text-align: right;
  height: 40upx;
  line-height: 40upx;
  padding-top: 4upx;
}
.tui-enclosure{
  display: flex;
  align-items: center;
  padding: 26upx 10upx;
  box-sizing: border-box;
}
.tui-mr{
  margin-right: 60upx;
}
.tui-cmt-btn{
  margin-top: 60upx;
}
</style>
