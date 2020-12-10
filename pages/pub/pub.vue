<template>
  <view>
    <!-- #ifdef APP-PLUS || H5 -->
    <uni-nav-bar left-icon="back" :statusBar="true" :border="false" left-text="返回" right-text="发布" @clickLeft="clickLeft" @clickRight="clickRight">
      <view class="uni-nav-bar_title" @click="showAction">
        <text class="uni-nav-bar_text">{{ selectTxt }}</text>
        <text class="iconfont icon-hanhan-01-01"></text>
      </view>
    </uni-nav-bar>
    <!-- #endif -->
    <!-- #ifdef MP-WEIXIN -->
    <uni-nav-bar  :border="false" right-text="发布" @clickLeft="clickLeft" @clickRight="clickRight">
      <view class="uni-nav-bar_title" @click="showAction">
        <text class="uni-nav-bar_text">{{ selectTxt }}</text>
        <text class="iconfont icon-hanhan-01-01"></text>
      </view>
    </uni-nav-bar>
    <!-- #endif -->
    
    <!-- 多行输入框 -->
    <view class="input-box">
      <textarea v-model="text"
      class="input-box_textarea" placeholder="说一句话吧～" />
    </view>
    
    <!-- 上传多图 -->
    <UploadImage :imageList="imageList" @filechange="handleFileChange"></UploadImage>
    
    <!-- 弹出公告 -->
    <uni-popup id="popup" ref="popup" type="center" :maskClick="false" :animation="false">
    	<view class="popup-content">
        <view class="popup-content_view_image">
          <image class="popup-content_image" src="/static/commom/tips.png" mode="widthFix"></image>
        </view>
        <view class="popup-content_view popup-content_title">
          <text>严禁发表以下信息</text>
        </view>
        <view class="popup-content_view">
          <text>1.涉及黄色，政治，广告及骚扰信息</text>
        </view>
        <view class="popup-content_view">
          <text>2.涉及人身攻击</text>
        </view>
        <view class="popup-content_view">
          <text>3.留联系方式，透露他人隐私</text>
        </view>
        <view class="popup-content_view">
          <text>一经核实将被封禁，情节严重者，永久封禁</text>
        </view>
        <view class="popup-content_view">
          <button type="default" hover-class="none" @click="toggle(false)">朕知道了</button>
        </view>
      </view>
    </uni-popup>
  </view>
</template>

<script>
  const selectOptions = ['所有人可见', '仅自己可见']
  import UploadImage from '@/components/upload-image.vue'
  export default {
    data() {
      return {
        selectTxt: '所有人可见',
        text: '',
        imageList: [],
        isShowTips: false, // 是否显示提示信息
      }
    },
    onReady() {
      uni.setNavigationBarTitle({
        title: '发布动态'
      })
      this.toggle(true)
    },
    // 监听返回事件
    onBackPress() {
      if (this.isShowTips || (!this.text && this.imageList.length === 0)) {
        return false // 返回false表示不阻止返回事件
      }
      uni.showModal({
        content: '是否要保存为草稿',
        cancelText: '不保存',
        confirmText: '保存',
        success: res => {
          this.isShowTips = true
          if (res.confirm) {
            // 保存
            console.log('===保存===')
          } else {
            // 不保存
            console.log('===不保存===')
          }
          uni.navigateBack({
            delta: 1
          })
        }
      });
      return true
    },
    methods: {
      // 监听上传文件变更
      handleFileChange (v) {
        this.imageList = v
      },
      // 显示下拉选项
      showAction() {
        uni.showActionSheet({
          itemList: selectOptions,
          success: res => {
            this.selectTxt = selectOptions[res.tapIndex]
          }
        })
      },
      // 提示信息显示/隐藏
      toggle(f) {
      	f ? this.$refs.popup.open() : this.$refs.popup.close()
      },
      // 点击发布按钮
      clickRight () {
        uni.showToast({
          title:'发布'
        })
      },
      // 点击返回按钮
      clickLeft() {
        uni.navigateBack({
          delta: 1
        })
      }
    },
    components: {
      UploadImage
    }
  }
</script>

<style lang="scss">
  // 头部css
  .uni-nav-bar_title {
    display: flex;
    flex: 1;
    justify-content: center;
    align-items: center;
  }

  .uni-nav-bar_text {
    font-size: 30rpx;
  }

  .icon-hanhan-01-01 {
    font-size: 30rpx;
  }
  
  // textarea
  .input-box {
    padding: $uni-spacing-row-sm;
  }
  .input-box_textarea {
    font-size: $uni-font-size-lg;
    color: $uni-color-title;
  }
  
  // 提示
  .popup-content {
    background-color: #FFFFFF;
    border-radius: 10rpx;
    width: 570rpx;
    padding: 30rpx;
    font-size: 30rpx;
  }
  .popup-content_view_image {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
  
  .popup-content_image {
    width: 100rpx;
    margin-bottom: 10rpx;
  }
  
  .popup-content_view {
    font-size: 30rpx;
    color: #171606;
    button {
      margin-top: 20rpx;
      background-color: #FFE934;
      color: #333333;
    }
  }
  .popup-content_title {
    color: #FFE934;
    text-align: center;
    font-size: 38rpx;
  }
</style>
