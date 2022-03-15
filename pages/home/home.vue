<template>
	<view class="bg">
		<view class="content">
			<u-input placeholder='password' v-model="password"></u-input>
			<u-button color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))" @click="getUser()">登录	
			</u-button>
		</view>
		<u-notify ref="uNotify" message="Hi uView"></u-notify>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				password: ''
			};
		},
		methods: {
			getUser() {
				if (this.password !== 'bb') {
					this.$refs.uNotify.show({
						top: 30,
						type: 'warning',
						color: '#fff',
						bgColor: '#CC0033',
						message: '密码错误',
						duration: 1000 * 3,
						fontSize: 20,
					})
					return
				}
				uni.getUserProfile({
					desc: 'getInfo',
					success: (res) => {
						uni.setStorageSync('userInfo', res.userInfo)
						uni.navigateTo({
							url:'../index/index'
						})
					},
					fail: () => {
						console.log("未授权");
					}
				})
			}
		}

	}
</script>

<style lang="scss">
.bg{
	height: 100vh;
	width: 100%;
	background: url(https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fi-1.33app.net%2F2019%2F9%2F12%2FKDYwMHgp%2F2050eb38-6614-48dc-80e6-50002d196cc0.jpg%3Fwidth%3D1080%26height%3D1920&refer=http%3A%2F%2Fi-1.33app.net&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1649916356&t=610b6ec817140d136e569962decb3bcd);
	background-size: contain;
	
}
.content{
		padding-top: 40%;
		width: 100%;
	}
</style>
