<template>
	<view class="content">
		<image src="../../static/file.png" class="img" mode="widthFix"></image>
		<u-button color="#83cbac" @tap='getUser'>点击获取授权</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			getUser(){
				uni.getUserProfile({
					desc: 'getInfo',
					success: (res) => {
						uni.setStorageSync('userInfo', res.userInfo)
						uni.navigateBack({
							
						})
					},
					fail: (err) => {
						console.log(err);
					}
				})
			}
		}
	}
</script>

<style scoped>
.content{
	text-align: center;
	padding-top:150rpx ;
}
.img{
	width: 200rpx;
	height: 200rpx;
	margin-bottom: 100rpx;
}
</style>
