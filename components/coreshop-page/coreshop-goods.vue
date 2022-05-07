<template>
    <view class="goods_list">
       <view class="goods_list_item" v-for="item in recommendListData" @click="goSigin(false)">
			<u-lazy-load 
			height="160"
			imgMode="scaleToFill"
			class="item_image" :image="item.thumbnail"></u-lazy-load>
			<view class="item_text_wrap">
				<view class="item_title u-line-2">
					{{item.title}}
				</view>
				<view class="item_time">
					{{item.startTime}}
				</view>
				<view class="item_address">
					{{item.activityaddress}}
				</view>
				<view class="item_bottom_bar">
					<view class="item_price">
						价格：<text>¥{{item.money}}</text>
					</view>
					<view class="item_haseen">
						{{item.amount}}人看过
					</view>
				</view>
			</view>
       </view>
    </view>
</template>

<script>
    import { goods} from '@/common/mixins/mixinsHelper.js'
    export default {
		mixins:[goods],
        name: "coreshopgoods",
        props: {
			recommendListData:{
				type:Array,
				default:[]
			}
        },
        data() {
            return {
                current: 0,
            };
        },
        computed: {
        },
        methods: {
        },
    }
</script>

<style scoped lang="scss">
	.goods_list{
		width: 100%;
		padding: 23rpx;
		margin-top: 15rpx;
		box-sizing: border-box;
		&_item{
			margin: 0 30rpx 31rpx 30rpx;
			padding: 20rpx 0;
			background-color: #2B323B;
			transform: skew(-20deg);
			box-shadow: 7rpx 8rpx 19rpx 2rpx rgba(0, 0, 0, 0.19);
			border-radius: 10rpx;
			display: flex;
			justify-content: space-between;
			.item_image{
				overflow: hidden;
				transform: skew(40deg);
				overflow: hidden;
				width: 220rpx;
				height: 160rpx;
				border-radius: 20rpx;
			}
			.item_text_wrap{
				transform: skew(20deg);
				flex: 1;
				margin-left: 46rpx;
				color: #fff;
				.item_title{
					font-size: 26rpx;
					overflow: hidden;
					font-weight: bold;
				}
				.item_time{
					margin-top: 13rpx;
				}
				.item_address{
					margin-top: 13rpx;
				}
				.item_bottom_bar{
					font-size: 20rpx;
					margin-top: 13rpx;
					display: flex;
					align-items: center;
					.item_price{
						text{
							color: #02B9EE;
							font-size: 30rpx;
						}
					}
					.item_haseen{
						margin-left: 90rpx;
					}
				}
			}
		}
	}
</style>
