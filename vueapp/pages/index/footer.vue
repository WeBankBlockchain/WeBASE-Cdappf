<template>
		<view  v-if="props.logo == 1" class="footer" :class="props.theme == 1 ? 'bg-white':''">
			<image src="../../static/logo-des.png" mode="widthFix"></image>
			<!-- <p class="footer-info">
				<span class="footer-info-span">由微众区块链、深圳大学微众金融科技实验室、深圳市金融区块链发展促进会提供技术支持</span>
			</p> -->
		</view>
		<view v-else class="footer" :class="props.theme == 1 ? 'bg-white':''">
			<image src="../../static/logo-suwiof.png" mode="aspectFit"></image>
			<p class="footer-info">由深圳大学微众金融科技实验室提供技术支持</p>
		</view>
</template>

<script lang="ts" setup>
	const props:any = defineProps({
		theme:{
			type:Number,
			default:() =>2 // 1白底  2灰底
		},
		logo:{
			type:Number,
			default:() =>2  // 1显示微众+深大  2只显示深大
		}
	})
	
</script>

<style lang="scss" scoped>
	.footer{
		text-align: center;
	}
	.footer image{
		height: 44rpx;
	}
	.footer-info{
		font-size: 24rpx !important;
		font-weight: 400;
		color: rgba(10,18,57,0.45) !important;
		line-height: 18px;
		margin-top: 20rpx;
	}
	.footer-info-span {
		margin:0 8rpx ;
	}
	.bg-white{
		background-color: white !important;
	}
</style>