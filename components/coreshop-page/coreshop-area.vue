<template>
	<view class="area-wrap">
		<view class="titleBg">
			<view>场地</view>
			<view class="zicheng"></view>
		</view>
		<view class="area-swipers">
			<swiper class="area-swipers" 
			easing-function="easeInOutCubic"
			:indicator-dots="false" :autoplay="false" :interval="1000" :duration="500">
				<swiper-item v-for="item in 3">
					<view class="area-swipers-top">
						<image src="" mode=""></image>
					</view>
					<view class="area-swipers-center">
						<image src="@/static/images/common/main_logo.png" mode="widthFix"></image>
					</view>
					<view class="area-swipers-center1">
						南山公馆
					</view>
					<view class="area-swipers-center_line">
						<image src="@/static/images/common/main_line.png" mode="widthFix"></image>
					</view>
					<view class="area-swipers-center_images">
						<view class="images_item" v-for="item in 3">
							<view class="gou">
								<image src="@/static/images/common/main_icon1.png" mode="widthFix"></image>
							</view>
							<view class="images">
								<image src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.jj20.com%2Fup%2Fallimg%2F4k%2Fs%2F02%2F2109242332225H9-0-lp.jpg&refer=http%3A%2F%2Fimg.jj20.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1653708791&t=db57545485baf03b586db7783aad9e48" mode="widthFix"></image>
							</view>
						</view>
					</view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	let that;
	export default{
		data(){
			return{
				list: [],
			}
		},
		props:{
			nearbyData1:{
                default: function () {
                    return []
                },
			}
		},
		watch:{
			nearbyData1:{
				handler(val){
					val.forEach(item => {
						that.list.push({
							image:item.imageUrl
						})
					})
				},
				deep:true
			}
		},
		mounted() {
		},
		created() {
			that = this;
			that.getCurrentLocation();
		},
		methods:{
			// 获取当前经纬度
			getCurrentLocation(){
				that.$emit("nearbyStoreList","TplIndexBanner1")
			}
		}
	}
</script>

<style lang="scss">
	.area-wrap{
		box-sizing: border-box;
		padding: 0 26rpx;
		.title{
			display: flex;
			align-items: center;
			width: 146rpx;
			line-height: 60rpx;
			height: 60rpx;
			display: block;
			text-align: center;
			color: #fff;
			font-size: 28rpx;
			background: url(../../static/images/indexMenus/titleBg.png) no-repeat;
			background-size: 100% 100%;
		}
		.zicheng{
			flex: 1;
		}
		.area-swipers{
			margin-top: 15rpx;
			height: 550rpx;
			width: 100%;
			background-color: #000;
			border-radius: 10rpx;
			padding-bottom: 20rpx;
			overflow: hidden;
			.area-swipers-top{
				width: 100%;
				height: 219rpx;
				image{
					width: 100%;
					height: 100%;
				}
			}
			.area-swipers-center{
				width: 112rpx;
				height: 112rpx;
				margin:-56rpx auto 0 auto;
				image{
					width: 100%;
					height: 100%;
				}
			}
			.area-swipers-center1{
				width: 180rpx;
				margin:7rpx auto 0 auto;
				text-align: center;
				font-size: 32rpx;
				font-weight: bold;
				color: #fff;
				letter-spacing: 3rpx;
			}
			.area-swipers-center_line{
				width: 100%;
				margin-top: -20rpx;
				image{
					width: 100%;
				}
			}
			.area-swipers-center_images{
				padding: 0 18rpx;
				display: flex;
				align-items: center;
				justify-content: space-between;
				.images_item{
					position: relative;
					width: 210rpx;
					height: 160rpx;
					.gou{
						z-index: 10;
						position: absolute;
						height: 72rpx;
						width: 29rpx;
						image{
							width: 100%;
							height: 100%;
						}
					}
					.images{
						overflow: hidden;
						position: absolute;
						width: 100%;
						height: 100%;
						left: 0;
						top: 0;
						image{
							width: 100%;
							height: 100%;
						}
					}
				}
				.images_item:last-child{
					.images{
						animation: jump1 3s infinite linear ;
					}
					.gou{
						top: -20rpx;
						right: 10rpx;
						transform: rotate(10deg) translateY(-10rpx);
					}
				}
				.images_item:nth-child(1){
					.images{
						animation: jump 3s infinite linear ;
					}
					.gou{
						top: -40rpx;
						left: 30rpx;
						transform: rotate(10deg) translateY(-10rpx);
					}
				}
				.images_item:nth-child(2){
					.images{
						animation: jump2 3s infinite linear ;
					}
					.gou{
						top: -20rpx;
						left: 50%;
						transform: translateX(-50%) scale(0.95);
					}
				}
			}
		}
	}
	
	@keyframes jump{
		0%{
			transform: rotate(10deg)  translateY(-10rpx);
		}
		50%{
			transform: rotate(13deg)  translateY(-10rpx);
		}
		100%{
			transform: rotate(10deg)  translateY(-10rpx);
		}
	}
	
	@keyframes jump1{
		0%{
			transform: rotate(-10deg)  translateY(-10rpx);
		}
		50%{
			transform: rotate(-13deg)  translateY(-10rpx);
		}
		100%{
			transform: rotate(-10deg)  translateY(-10rpx);
		}
	}
	
	@keyframes jump2{
		0%{
			transform: rotate(0deg)  translateY(5rpx) scale(0.95);
		}
		25%{
			transform: rotate(-3deg)  translateY(5rpx) scale(0.95);
		}
		50%{
			transform: rotate(0deg)  translateY(5rpx) scale(0.95);
		}
		75%{
			transform: rotate(3deg)  translateY(5rpx) scale(0.95);
		}
		100%{
			transform: rotate(0deg)  translateY(5rpx) scale(0.95);
		}
	}
</style>
