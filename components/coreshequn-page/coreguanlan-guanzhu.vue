<template>
	<view class="coreshequnMygroup">
		<view class="group_guanzhu">
			<swiper
			class="swipers"
			:display-multiple-items="6"
			:acceleration="true"
			:circular="true"
			:indicator-dots="false" :autoplay="false" :interval="0" :duration="3000">
				<swiper-item v-for="item in 6">
					<view class="group_guanzhu_item">
						<u-image 
						width="100%"
						height="90rpx"
						:src="require('@/static/images/common/test_pic.png')">
							<u-loading slot="loading"></u-loading>
						</u-image>
						<u-icon 
						class="guanzhu"
						:name="require('@/static/images/common/guanzhu.png')" 
						size="30" color="#02B9EE"></u-icon>
					</view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	export default{
		
	}
</script>

<style lang="scss" scoped>
	.coreshequnMygroup{
		.group_guanzhu{
			border-bottom: 2rpx solid #61676F;
			padding-bottom: 27rpx;
			width: 100%;
			margin-top: 20rpx;
			.swipers{
				height: 90rpx;
			}
			.group_guanzhu_item{
				width: 90rpx;
				height: 90rpx;
				padding-bottom: 20rpx;
				background: #2B323B;
				border-radius: 50%;
				position: relative;
				.guanzhu{
					position: absolute;
					right: 0;
					bottom: 0;
				}
			}
		}
	}
</style>
