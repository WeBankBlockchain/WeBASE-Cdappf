<template>
	<view class="content">
		<view class="banner">
			<image :src="queryPageData.banner"></image>
		</view>
		<view class="nav">
			<view class="organize">
				<image src="/static/icon-org.png" mode="aspectFit"></image>
				<span>发证机构：{{queryPageData.certOrg}}</span>
			</view>
			<view class="search-input-view">
				<image src="/static/icon-search.png" mode="heightFix" @click="getData"></image>
				<input class="search-input" v-model="searchUserId" @confirm="getData"
					:placeholder="queryPageData.searchHint" />
			</view>
			<view class="result-noResult" v-if="noResultShow">
				<view class="info-no">未能搜索到您的证书</view>
			</view>
			<view class="certList" v-if="resultShow&&this.certList.length>0">
				<view class="certItem" @click="goDetail(item)" v-for="(item,index) in this.certList">
					<image src="/static/icon_certificate.svg"></image>
					<span>{{item.title}}</span>
				</view>
			</view>
		</view>
		<view class="logo"><img :src="queryPageData.bottomLog" /></view>
		<view class="support">
			<span>{{queryPageData.explainDoc}}</span>
		</view>
		<veCode v-if="veCodeShow" @closeCodePage="closeVeCode" :searchData="searchUserId" :certificate_Id="certificate_Id" @success="showList"></veCode>
	</view>
</template>

<script>
	import veCode from './veCode.vue'
	import {
		uuid,
		MD5
	} from '../../components/common.js'
	export default {
		components: {
			veCode
		},
		data() {
			return {
				searchUserId: "",
				resultShow: false,
				noResultShow: false,
				veCodeShow: false,
				certificate_Id: null,
				queryPageData: {
					banner: '',
					bottomLog: '',
					certOrg: '',
					explainDoc: '',
					searchHint: '',
				},
				certList: []
			}
		},
		onLoad() {
			let urlStr = location.href.split('?')[1]
			const urlSearchParams = new URLSearchParams(urlStr)
			const result = Object.fromEntries(urlSearchParams.entries())
			console.log(result)
			if (!result.Id) {
				uni.showToast({
					icon: 'none',
					title: '未找到配置Id'
				})
				return;
			}
			this.certificate_Id = result.Id
			this.getQueryPageData()
		},
		methods: {
			getQueryPageData() {
				const reqData = {
					certConfigId: this.certificate_Id,
				};
				uni.request({
					url: "/certapp-cert/cert/getQueryPage",
					data: reqData,
					header: {
						'bizSeqNo': uuid() //自定义请求头信息
					},
					success: (res) => {
						if (res.data.code === 0) {
							if (res.data.result) {
								this.queryPageData = res.data.result;
							} else {
								uni.showToast({
									icon: 'none',
									title: '请求数据异常'
								})
							}
						} else {
							uni.showToast({
								icon: 'none',
								title: '系统繁忙请稍后再试'
							})
						}
					},
					fail(err) {
						console.info(err)
						uni.showToast({
							icon: 'none',
							title: '系统繁忙请稍后再试'
						})
					}
				})
			},
			closeVeCode() {
				this.veCodeShow = false
			},
			showList(list) {
				this.veCodeShow = false
				this.certList = list
				if (this.certList && this.certList.length > 0) {
					this.resultShow = true
					this.noResultShow = false
				} else {
					this.noResultShow = true
					this.resultShow = false
				}
			},
			getData() {
				const {
					searchUserId = ""
				} = this;
				if (!searchUserId) {
					uni.showToast({
						icon: 'none',
						title: this.queryPageData.searchHint
					})
					return;
				}
				this.veCodeShow = true
			},
			goDetail(certItem) {
				if (certItem) {
					uni.navigateTo({
						url: './detail?data=' + encodeURIComponent(JSON.stringify(certItem))
					});
				} else {
					uni.showToast({
						icon: 'none',
						title: '证书不能为空'
					})
					return;
				}
			}
		},

	}
</script>

<style scoped lang="scss">
	.content {
		font-family: PingFang SC;

		.banner {
			width: 100%;
			z-index: 0 !important;

			image {
				width: 100%;
				z-index: 0 !important;
			}
		}

		.nav {
			position: relative;
			background-color: #fff;
			z-index: 99;
			border-radius: 40rpx 40rpx 0rpx 0rpx;
			margin-top: -40rpx;
			padding: 48rpx;

			.organize {
				width: 100%;
				margin: 0 auto;
				display: flex;
				flex-wrap: wrap;
				justify-content: center;
				color: #787B84;
				font-size: 28rpx;
				line-height: 44rpx;
				display: flex;
				flex-wrap: nowrap;
				margin-bottom: 48rpx;

				span {
					display: inline-block;
					word-break: break-all;
				}

				image {
					width: 44rpx;
					height: 44rpx;
					margin-right: 12rpx;
				}
			}

			.result-noResult {
				.info-no {
					width: 100%;
					height: 22px;
					margin-top: 25px;
					font-family: 'PingFang SC';
					font-style: normal;
					font-weight: 400;
					font-size: 14px;
					line-height: 22px;
					text-align: center;
					color: #787B84;
				}
			}

			.certList {
				margin-top: 30px;

				.certItem {
					margin-top: 15px;

					image {
						width: 24px;
						height: 24px;
						margin-right: 8px;
						vertical-align: middle;
					}

					span {
						color: #336CC2;
						font-family: PingFang SC;
						font-size: 15px;
						font-style: normal;
						font-weight: 500;
						line-height: 22px;
						/* 146.667% */
					}
				}
			}
		}

		.result {
			height: calc(100vh - 930rpx);
		}

		.result.active {
			height: calc(100vh - 1026rpx);
		}

		.result-container {
			padding-top: 30px;
			padding-bottom: 20px;
		}

		.result-info {
			color: #336CC2;
			width: 100%;
			line-height: 26px;
			padding: 4px;

			.reward-img {
				width: 22px;
				height: auto;
				float: left;
				margin-right: 6px;
				transform: translateY(2px);

			}
		}

		.result-info-no {
			position: absolute;
			width: 100%;
			height: 22px;
			top: 25px;
			font-family: 'PingFang SC';
			font-style: normal;
			font-weight: 400;
			font-size: 14px;
			line-height: 22px;
			text-align: center;
			color: #787B84;
		}

		.search-input-view {
			position: relative;
			width: 100%;
			height: 102rpx;
		}

		.search-input-view image {
			position: absolute;
			right: 22rpx;
			top: 22rpx;
			width: 56rpx;
			height: 56rpx;
		}

		.search-input {
			width: 100%;
			height: 102rpx;
			border-radius: 16rpx;
			border: 4rpx solid #336CC2;
			padding: 0 100rpx 0 22rpx;
			box-sizing: border-box;
		}

		.input-placeholder {
			color: #BFC3CC !important;
		}

		.support {
			padding: 0 32px;
			position: absolute;
			bottom: 50px;
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
			bottom: 75px;
			width: 100%;
			text-align: center;
			box-sizing: border-box;

			img {
				height: 26px;
				width: auto;
				background-repeat: no-repeat;
			}
		}
	}
</style>