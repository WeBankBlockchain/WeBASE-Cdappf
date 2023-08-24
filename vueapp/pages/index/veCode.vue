<template>
	<view class="verificationCode">
		<view tabindex="-1" class="content-tip" style="z-index: 888;">
			<view class="content-box">
				<view class="content-box__header">
					<img class="content-box__headerbtn" src="/static/close.svg" @click="closeTip" />
				</view>
				<view class="content-box__icon">
					请输入图中验证码
				</view>
				<view class="content-box__content">
					<input class="search-input" v-model="code" placeholder="请输入验证码" />
					<image :src="codeImg" class="codeImg" @click="getCode"></image>
				</view>
				<view class="content-box__btns" @click="queryCert">
					确定
				</view>
			</view>
		</view>
		<view class="v-modal" tabindex="0" style="z-index: 666;"></view>
	</view>
</template>

<script>
	import {
		uuid,
		MD5
	} from '../../components/common.js'
	export default {
		props: ["searchData","certificate_Id"],
		data() {
			return {
				certList: [],
				code: '',
				codeImg: ''
			}
		},
		mounted() {
			this.getCode()
		},
		methods: {
			getCode() {
				uni.request({
					url: "/certapp-cert/pictureCheckCode",
					success: (res) => {
						if (res.data.code === 0) {
							if (res.data.result) {
								this.codeImg = res.data.result;
							} else{
								uni.showToast({
									icon: 'none',
									title: '查询验证码异常'
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
			queryCert() {
				const {
					code = ""
				} = this;
				if (!code) {
					uni.showToast({
						icon: 'none',
						title: '请输入验证码'
					})
					return;
				}
				//获取用户证书列表
				const reqData = {
					checkCode: this.code,
					filedInfo: this.searchData,
					certConfigId: this.certificate_Id
				}
				uni.request({
					url: "certapp-cert/queryFuzzyFiledInfo",
					data: reqData,
					header: {
						'bizSeqNo': uuid() //自定义请求头信息
					},
					success: (res) => {
						if (res.data.code === 0) {
							if (res.data.result) {
								this.certList = res.data.result;
								this.$emit('success', this.certList)
							}
						} else {
							uni.showToast({
								icon: 'none',
								title: '系统繁忙请稍后再试'
							})
						}
						this.getCode()
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
			closeTip() {
				this.$emit('closeCodePage')
			}
		}

	}
</script>

<style scoped lang="scss">
	.verificationCode {
		width: 100%;
		height: 100%;
		position: relative;
		background-color: pink;
		background-repeat: no-repeat;
		background-size: 100% 100%;

		.v-modal {
			position: fixed;
			left: 0;
			top: 0;
			width: 100%;
			height: 100%;
			opacity: 0.5;
			background: #000;
		}

		.content-tip {
			position: fixed;
			top: 0;
			bottom: 0;
			left: 0;
			right: 0;
			text-align: center;

			.content-box__headerbtn {
				position: absolute;
				width: 15px;
				top: 20px;
				right: 20px;
				padding: 0;
				border: none;
				outline: 0;
				background: 0 0;
				font-size: 16px;
				cursor: pointer;
			}

			.content-box {
				display: inline-block;
				width: 300px;
				padding: 32px 24px;
				vertical-align: middle;
				border-radius: 8px;
				background: #FFF;
				text-align: center;
				position: relative;

				.content-box__icon {
					color: var(--grey-1, #262A32);
					text-align: center;
					/* header 18 */
					font-family: PingFang SC;
					font-size: 18px;
					font-style: normal;
					font-weight: 500;
					line-height: 26px;
					/* 144.444% */
				}

				.content-box__content {
					margin: 32px 0;

					display: flex;

					height: 44px;
					padding: 1px 1px 1px 8px;
					justify-content: flex-end;
					align-items: center;
					box-sizing: border-box;
					border: 1px solid var(--grey-5, #E0E4ED);

					.search-input {
						width: 70%;
					}

					image {
						width: 30%;
						height: 42px;
						border-left: 1px solid var(--grey-5, #E0E4ED);
					}

				}

				.content-box__btns {
					display: flex;
					padding: 10px 40px;
					justify-content: center;
					align-items: center;
					gap: 10px;
					align-self: stretch;
					border-radius: 8px;
					background: #336CC2;
					color: #FFF;
					text-align: center;
					/* 16 Medium */
					font-family: PingFang SC;
					font-size: 16px;
					font-style: normal;
					font-weight: 500;
					line-height: 24px;
					/* 150% */
				}
			}
		}

		.content-tip::after {
			content: '';
			display: inline-block;
			height: 100%;
			width: 0;
			vertical-align: middle;
		}
	}
</style>