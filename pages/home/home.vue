<template>
	<view class="bg">
		<view class="content">
			<u-input placeholder='username' color='#fff' v-model="username"></u-input>
			<u-input placeholder='password' color='#fff' v-model="password" type='password'></u-input>
			<u-button color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))" @click="getUser()">登录
			</u-button>
			<u-toast ref="uToast"></u-toast>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				password: ''
			};
		},
		methods: {
			getUser() {
				if (this.username === '') {
					this.$refs.uToast.show({
						type: 'error',
						icon: false,
						message: '用户名为空',
					})
					return
				}

				if (this.password === '') {
					this.$refs.uToast.show({
						type: 'error',
						icon: false,
						message: '密码为空',
					})
					return
				}

				this.$db.collection("user").where({
					username: this.username
				}).get().then(res => {
					let user = res.data;
					console.log(user.length);
					if (res.data.length === 0) {
						this.$refs.uToast.show({
							type: 'error',
							icon: false,
							message: '用户不存在',
						})
						return
					} else if (user[0].password !== this.password) {
						this.$refs.uToast.show({
							type: 'error',
							icon: false,
							message: '密码错误',
						})
						return
					} else {
						uni.switchTab({
							url: '/pages/index/index',
							success(res) {},
							fail(err) {
								// errMsg: "navigateTo:fail page "pages/index/pages/test/test" is not found"
							}
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.bg {
		height: 100vh;
		width: 100%;
		background: url(https://636c-cloud1-3gkkta0j361cad06-1310095590.tcb.qcloud.la/img/1647681261777-9830?sign=d9441e205651913c04d395570801bd83&t=1647681294);
		background-size: contain;
	}

	.content {
		padding-top: 90%;
		width: 100%;
	}
</style>
