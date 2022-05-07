<template>
	<view class="coreshequnMygroup">
		<view class="group_title">
			<view class="titleBg">
				我的小组
			</view>
			<view class="more">
				<text>更多</text>
				<u-icon name="arrow-right" size="20" color="#02B9EE"></u-icon>
			</view>
		</view>
		<view class="group_swiper">
			<swiper class="group_swiper" 
			:display-multiple-items="3.5"
			:acceleration="true"
			:circular="true"
			:indicator-dots="false" :autoplay="true" :interval="0" :duration="3000">
				<swiper-item v-for="item in 5">
					<view class="group_swiper_item">
						<view class="group_top">
							<view class="group_pic">
								<u-lazy-load 
								:image="require('@/static/images/common/test_pic.png')"></u-lazy-load>
							</view>
							<view class="group_name u-line-1">
								<u-icon></u-icon>
								<text>花非花雾非雾</text>
							</view>
						</view>
						<view class="group_bar u-line-1">
							<text>2459人参与互动</text>
							<u-icon name="arrow-right-double"></u-icon>
						</view>
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
		.group_title{
			display: flex;
			justify-content: space-between;
			align-items: center;
			.more{
				font-size: 24rpx;
				color: #02B9EE;
			}
		}
		.group_swiper{
			width: 100%;
			margin-top: 20rpx;
			height: 203rpx !important;
			.group_swiper_item{
				margin-right: 26rpx;
				width: 180rpx;
				background: #2B323B;
				box-shadow: 4rpx 4rpx 8rpx 0rpx rgba(213, 213, 213, 0.03);
				border-radius: 5rpx;
				overflow: hidden;
				.group_top{
					height: 150rpx;
					overflow: hidden;
					.group_pic{
						width: 66rpx;
						height: 66rpx;
						border-radius: 50%;
						overflow: hidden;
						margin: 13rpx auto 23rpx auto;
					}
					.group_name{
						padding: 0 13rpx;
						font-size: 15rpx;
						color: #fff;
						display: flex;
						align-items: center;
						justify-content: center;
					}
				}
				.group_bar{
					display: flex;
					justify-content: space-between;
					align-items: center;
					height: 50rpx;
					line-height: 50rpx;
					padding:0 13rpx;
					box-sizing: border-box;
					background-color: #D5D5D5;
					font-size: 18rpx;
					color: #fff;
					text{
						margin-right: 10rpx;
					}
				}
			}
		}
	}
</style>
