<template>
	<view class="u-page" style="padding: 30rpx;">
		<view class="u-demo-block">
			<view class="u-demo-block__content">
				<view class="album" v-for="(item,index) in list" :key='index'>
					<view class="album__avatar">
						<image :src="item.avatarUrl" mode="" style="width: 32px;height: 32px;"></image>
					</view>
					<view class="album__content">
						<u--text :text="item.creater" type="primary" bold size="17"></u--text>
						<u--text margin="0 0 8px 0" :text="item.content"></u--text>
						<u-album :urls="item.urls"></u-album>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				albumWidth: 0,
				list: [],
				userInfo: {
					nickName: ''
				}
			}
		},
		methods: {
			getData() {
				this.$db.collection('list').get().then(res => {
					console.log(res.data);
					this.list = res.data
				})
			}
		},
		onPullDownRefresh: function() {
			console.log('onPullDownRefresh')
			this.getData()
		},
		onLoad() {
			if (uni.getStorageSync('userInfo')) {
				this.getData()
			} else {
				uni.navigateTo({
					url: '../home/home'
				})
			}
		}
	}
</script>

<style lang="scss">
	.album {
		@include flex;
		align-items: flex-start;

		&__avatar {
			background-color: $u-bg-color;
			padding: 5px;
			border-radius: 3px;
		}

		&__content {
			margin-left: 10px;
			flex: 1;
		}
	}
</style>
