<template>
	<view class="u-page">
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
						<view class="flex">
							<u--text :text="item.time" size="12"></u--text>
							<u--text style="margin-left:20rpx;" text="删除" @tap='del(item._id)' type="primary" bold size="17"></u--text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<u-back-top mode="square" :scroll-top="scrollTop"></u-back-top>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				albumWidth: 0,
				scrollTop: 0,
				list: [],
				userInfo: {
					nickName: ''
				}
			}
		},
		onPageScroll(e) {
				this.scrollTop = e.scrollTop;
		},
		methods: {
			getData() {
				this.$db.collection('list').orderBy('time','desc').get().then(res => {
					console.log(res.data);
					this.list = res.data;
				})
			},
			del(id) {
				this.$db.collection('list').doc(id).remove().then(res => {
					this.getData();
				})
			}
		},
		onPullDownRefresh: function() {
			console.log('onPullDownRefresh')
			this.getData()
		},
		onShow() {
			console.log('getlists');
			if (uni.getStorageSync('userInfo').nickName !== '') {
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
	.u-page{
		padding: 50rpx;
	}
	.u-album{
		text-align: center;
	}
	.album {
	        @include flex;
	        align-items: flex-start;
			margin-bottom: 10rpx;
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
	/deep/.album__avatar{
		width: 32px;
		height: 32px;
		padding: 0px;
	}
	.flex {
		display: flex;
		justify-content: space-between;
	}
</style>
