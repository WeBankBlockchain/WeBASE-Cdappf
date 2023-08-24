<template>
	<view class="content">
		<view class="face">
			<view class="back" @click="back">
				<image mode="widthFix" src="/static/icon_back.png" alt="" />
			</view>
			<view class="certTitle">{{certData.title}}</view>
			<image class="face-bg" mode="widthFix" src="/static/bg_certificate.png"></image>
			<!-- <image class="face-line" src="/static/line.png"></image> -->
			<image lazy-load="true" show-menu-by-longpress="true" mode="widthFix" class="face-img"
				:src="certData.picPreview">
			</image>
		</view>
		<view class="detail-info">
			<p class="detail-info-p" v-for="(item, index) in certData.enableFields" :key="index">
				<span class="title">{{ JSON.parse(item).filedNameZh }}</span>
				<span class="item-value">{{ JSON.parse(item).filedDemo }}</span>
			</p>
		</view>
		<view class="logo"><img :src="certData.bottomLog" /></view>
		<view class="support">
			<span>{{certData.explainDoc}}</span>
		</view>
		<!-- <view class="share-btn">
			<button type="primary" class="primary-btn" open-type="share" @click="share">分享</Button>
		</view> -->
	</view>
</template>

<script>
	import Footer from './footer.vue'
	import {
		uuid,
		MD5
	} from '../../components/common.js'
	export default {
		components: {
			Footer
		},
		data() {
			return {
				certData: {
					title: "",
					certConfigId: "",
					enableFields: [],
					bottomLog: "",
					explainDoc: "",
					picPreview: "",
				}
			}
		},
		onLoad(option) {
			const data = JSON.parse(decodeURIComponent(option.data))
			this.certData = data
		},
		methods: {
			back() {
				if (this.pagesLength == 1) {
					uni.navigateTo({
						url: './index'
					});
				} else {
					uni.navigateBack({
						delta: 1
					});
				}
			},
		}
	}

	// 分享
	// const onShareAppMessage = (res) => {
	// 	return {
	// 		title: '微众链证书',
	// 		url: '/pages/index/detail',
	// 		success(res) {
	// 			uni.showToast({
	// 				title: '分享成功'
	// 			})
	// 		},
	// 		fail(res) {
	// 			uni.showToast({
	// 				title: '分享失败',
	// 				icon: 'none'
	// 			})
	// 		}
	// 	}
	// }
</script>
<style>
	page {
		background-color: #f3f3f3;
	}
</style>
<style lang="scss" scoped>
	.content {
		overflow-y: auto;
		height: 100%;
		padding-bottom: 60rpx;


		.support {
			padding: 0 32px;
			position: absolute;
			bottom: 60px;
			width: 100%;
			text-align: center;
			color: #999;
			font-family: PingFang SC;
			font-size: 12px;
			font-style: normal;
			font-weight: 400;
			line-height: normal;
			transform: scale(0.83);
			box-sizing: border-box;
		}

		.logo {
			padding: 0 32px;
			position: absolute;
			bottom: 90px;
			width: 100%;
			text-align: center;
			box-sizing: border-box;

			img {
				height: 20px;
				width: auto;
				background-repeat: no-repeat;
			}
		}

		.face {
			width: 100%;
			text-align: center;
			position: relative;
			height: auto;

			.certTitle {
				position: absolute;
				top: 12px;
				width: 100%;
				text-align: center;
				color: #262A32;
				text-align: center;
				font-family: PingFang SC;
				font-size: 15px;
				font-style: normal;
				font-weight: 600;
				line-height: 22px;
				z-index: 666
					/* 146.667% */
			}

			.back {
				position: absolute;
				top: 12px;
				left: 16px;
				z-index: 999;
				width: 25px;

				image {
					width: 100%;
					height: auto;
				}
			}

			&-bg {
				width: 100%;
				height: auto;
			}

			&-line {
				width: 60%;
				margin: 20rpx 0;
				height: 10rpx;
				position: relative;
				z-index: 3 !important;
			}

			&-img {
				position: absolute;
				top: 25%;
				left: 17%;
				width: 66%;
				height: auto;
				z-index: 3;
			}

		}
	}

	.detail-info {
		background-color: #fff;
		position: relative;
		margin-top: -40rpx;
		border-radius: 40rpx;
		padding: 32rpx 64rpx 16rpx 64rpx;
		margin-bottom: 32rpx;
		min-height: 300px;

		.detail-info-p {
			margin-bottom: 16rpx;
			display: flex;
			flex-wrap: nowrap;
		}

		.title {
			color: rgb(82, 86, 89);
			font-size: 24rpx;
			line-height: 40rpx;
			margin-right: 56rpx;
			width: 108rpx;
			word-break: break-all;
			display: inline-block;
			text-align: left;
		}

		.title.after {
			content: "";
			display: inline-block;
			min-width: 108rpx;
		}

		.item-value {
			color: rgb(82, 86, 89);
			font-size: 24rpx;
			line-height: 40rpx;
			flex: 1;
			text-align: left;
			word-break: break-all;
		}
	}

	.share-btn {
		display: flex;
		flex-wrap: nowrap;
		position: absolute;
		width: 100%;
		bottom: 0;
		left: 0;
		text-align: center;
		justify-content: center;
		// height: 136rpx;
		justify-items: center;
		padding: 24rpx 0rpx;
		z-index: 9;
		background-color: #fff;

		.primary-btn {
			margin: 0 auto;
			height: 88rpx;
			padding: 0;
			text-align: center;
			line-height: 88rpx;
			width: 670rpx;
			border-radius: 16rpx;
			background-color: #336CC2;
		}
	}
</style>