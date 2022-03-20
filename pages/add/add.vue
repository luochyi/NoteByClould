<template>
	<view class="content">
		<u--textarea autoHeight v-model="form.content" placeholder="记录一条..."></u--textarea>
		<u-icon name="photo" color="#2979ff" @tap='addImg' size="38"></u-icon>
		<view class="u-page">
			<view class="u-demo-block">
				<view class="u-demo-block__content">
					<view class="album">
						<view class="album__content">
							<u-album :urls="urls"></u-album>
						</view>
					</view>
				</view>
			</view>
		</view>
		<u-button type="primary" text="确定" @click="submit"></u-button>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				img: '',
				albumWidth: 0,
				urls: [],
				form: {
					content: ''
				}
			}
		},
		onHide() {
			console.log(1);
			this.urls = [];
			this.form.content ='';
		},
		methods: {
			submit() {
				console.log(uni.getStorageSync('userInfo').nickName);
				if(uni.getStorageSync('userInfo').nickName===null||uni.getStorageSync('userInfo').nickName===undefined){
					this.$refs.uToast.show({
						type: 'error',
						icon: false,
						message: '请先授权',
					})
					uni.navigateTo({
						url:'../get/get'
					})
					return
				}
				
				console.log(uni.getStorageSync('userInfo'));
				console.log(this.formatDate());
				this.$db.collection('list').add({
					 data: {
						time:this.formatDate(),
					    urls:this.urls,
						content:this.form.content,
						creater:uni.getStorageSync('userInfo').nickName,
						avatarUrl:uni.getStorageSync('userInfo').avatarUrl
					  },
					  success: function(res) {
					    // res 是一个对象，其中有 _id 字段标记刚创建的记录的 id
					    console.log(res)
						
						uni.switchTab({
							url:'/pages/index/index',
							success(res) {
							console.log('回首页');
							},
							fail(err) {
							// errMsg: "navigateTo:fail page "pages/index/pages/test/test" is not found"
							console.log(err);
							}
						})
					  },
					  fail: (err) => {
					  	console.log(err)
					  }
				})
			},
			formatDate() {
			    let date = new Date();
			    let year = date.getFullYear(); // 年
			    let month = date.getMonth() + 1; // 月
			    let day = date.getDate(); // 日
			    let week = date.getDay(); // 星期
			    let weekArr = ["星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"];
			    let hour = date.getHours(); // 时
			    hour = hour < 10 ? "0" + hour : hour; // 如果只有一位，则前面补零
			    let minute = date.getMinutes(); // 分
			    minute = minute < 10 ? "0" + minute : minute; // 如果只有一位，则前面补零
			    let second = date.getSeconds(); // 秒
			    second = second < 10 ? "0" + second : second; // 如果只有一位，则前面补零
			    let nowDate = `${year}.${month}月${day}日 ${hour}:${minute}`;
				return nowDate
			   },
			addImg: function() {
				wx.chooseImage({ //选择图片
					count: 9, //规定选择图片的数量，默认9
					sizeType: ["original", "compressed"], //规定图片的尺寸， 原图/压缩图
					sourceType: ['album', 'camera'], //从哪里选择图片， 相册/相机
					success: (chooseres) => { //接口调用成功的时候执行的函数
						console.log(chooseres);
						chooseres.tempFilePaths.forEach((ele, i) => {
							wx.cloud.uploadFile({
								cloudPath: "img/" + new Date().getTime() + "-" + Math
									.floor(Math
										.random() * 1000) + i, //云储存的路径及文件名
								filePath: ele, //要上传的图片/文件路径 这里使用的是选择图片返回的临时地址
								success: (res) => { //上传图片到云储存成功
									console.log(res);
									this.urls.push(res.fileID)
								},
								fail: (err) => {
									console.log(err)
								}
							})
						})
					},
					fail: (err) => {
						console.log(err)
					}
				})
			}
		}
	}
</script>
<style lang="scss">
	.content {
		padding: 20rpx;
	}

	.upload {
		width: 100rpx;
		height: 100rpx;
		background-color: #BBBCBE;
	}
</style>
