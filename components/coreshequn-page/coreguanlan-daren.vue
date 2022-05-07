<template>
	<view class="coreshequnMygroup">
		<view class="group_title">
			<view class="titleBg">
				观澜达人
			</view>
			<!-- <view class="more">
				<text>更多</text>
				<u-icon name="arrow-right" size="20" color="#02B9EE"></u-icon>
			</view> -->
		</view>
		<view class="group_swiper">
			<swiper
			class="swipers"
			:display-multiple-items="3.2"
			:acceleration="true"
			:circular="true"
			:indicator-dots="false" :autoplay="false" :interval="0" :duration="3000">
				<swiper-item v-for="item in 5">
					<view class="group_swiper_item">
						<view class="group_top">
							<u-image 
							height="150rpx"
							:src="require('@/static/images/seller/test.png')">
								<u-loading slot="loading"></u-loading>
							</u-image>
						</view>
						<view class="group_pic">
							<u-image 
							width="100%"
							height="54rpx"
							:src="require('@/static/images/common/test_pic.png')">
								<u-loading slot="loading"></u-loading>
							</u-image>
						</view>
						<view class="group_name">小羊肖恩</view>
						<view class="group_bar">
							<view class="left_group_bar">
								<u-icon name="heart" size="25" color="#D92626"></u-icon>
								2.8w
							</view>
							<view class="guanzhu">
								＋关注
							</view>
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
			margin-top: 27rpx;
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
			.swipers{
				height: 275rpx;
			}
			.group_swiper_item{
				width: 180rpx;
				padding-bottom: 20rpx;
				background: #2B323B;
				border-radius: 5rpx;
				.group_top{
					height: 150rpx;
					overflow: hidden;
				}
				.group_pic{
					width: 54rpx;
					height: 54rpx;
					margin-left: 10rpx;
					border-radius: 50%;
					overflow: hidden;
					margin-top: -27rpx;
				}
				.group_name{
					margin-top: 10rpx;
					margin-bottom: 20rpx;
					padding: 0 10rpx;
					color: #fff;
					font-size: 20rpx;
				}
				.group_bar{
					display: flex;
					justify-content: space-between;
					align-items: center;
					padding: 0 10rpx;
					.left_group_bar{
						display: flex;
						align-items: center;
						color: #5D637C;
						font-size: 20rpx;
					}
					.guanzhu{
						width: 63rpx;
						text-align: center;
						line-height:23rpx;
						color: #fff;
						font-size: 16rpx;
						height: 23rpx;
						background: #02B9EE;
						border-radius: 5rpx;
					}
				}
			}
		}
	}
</style>
