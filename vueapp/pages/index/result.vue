<template>
	<view class="content" scroll-y alwaysScrollableVertical="true">
		<view class="no-result" v-if="!result || result.length == 0">
			<image src="/static/icon-no-result.png"></image>
			<p class="no-result-p">未能搜索到您的证书</p>
		</view>
		<!-- <view class="back-btn">
			<button type="primary" class="btn" open-type="share" @click="back">返回</Button>
		</view> -->
		<Footer :theme="1" :logo="2" class="foot" />
	</view>
</template>

<script>
	import Footer from './footer.vue'
	export default {
		components: {
			Footer
		},
		data() {
			return {
				result: []
			}
		},
		onLoad() {},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			}
		}

	}
</script>

<style scoped lang="scss">
	.content {
		padding: 48rpx;
		height: 100%;
		.back {
			position: absolute;
			top: 9px;
			left: 16px;
			font-size: 26px;
			z-index: 999;
			color: #333;
		}

		.no-result {
			height: calc(100vh - 872rpx);
			display: flex;
			flex-direction: column;
			width: 100%;
			align-items: center;
			justify-items: center;
			justify-content: flex-end;

			image {
				width: 240rpx;
				height: 240rpx;
				margin-bottom: 40rpx;
			}

			.no-result-p {
				font-size: 24rpx;
				color: #787B84;
			}
		}

		.result {
			height: calc(100vh - 336rpx);
			overflow-y: auto;
		}

		.result-info {
			font-size: 32rpx;
			font-weight: 400;
			width: 100%;
			color: #315CE8 !important;
			line-height: 48rpx;
		}

		.result-infohover {
			opacity: 0.8;
		}
	}

	.back-btn {
		margin-top: 64rpx;

		button {
			width: 224rpx;
			height: 88rpx;
			background: #336CC2;
			border-radius: 16rpx;
		}
	}

	.foot {
		position: absolute;
		bottom: 100rpx;
	}
</style>
