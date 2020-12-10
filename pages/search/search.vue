<template>
	<view class="search">
		<!-- #ifdef MP-WEIXIN -->
		<view class="search-top">
		  <input class="search_input" type="text" v-model="text" placeholder="搜索糗事" />
      <text class="search_text">取消</text>
		</view>
		<!-- #endif -->
    搜索
	</view>
</template>

<script>
	export default {
		data() {
			return {
				text: ''
			}
		},
    onReady() {
      uni.setNavigationBarTitle({
        title: '搜索糗事'
      })
    },
    // 监听原生标题栏按钮点击事件
    onNavigationBarButtonTap(e) {
      if (e.index === 0) {
        uni.navigateBack({
          delta: 1
        })
      }
    },
    // 监听搜索框内容变化
    onNavigationBarSearchInputChanged(e) {
      console.log(e.text)
    },
    // 监听键盘搜索按钮点击事件
    onNavigationBarSearchInputConfirmed(e) {
      console.log(e.text)
    },
		methods: {
			
		}
	}
</script>

<style>
  /* #ifdef MP-WEIXIN */
  .search-top {
    flex-direction: row;
    display: flex;
    padding: 10rpx 20rpx;
  }
  .search_input {
    flex: 1;
    background-color: #F7F7F7;
    height: 60rpx;
    border-radius: 12rpx;
    color: #666666;
    padding: 0 10rpx;
  }
  .search_text {
    font-size: 32rpx;
    color: #666666;
    padding-left: 10rpx;
  }
  /* #endif */

</style>
