<template>
	<view class="coreshequnMygroup">
		<view class="group_title" v-if="showTitle">
			<view class="titleBg mySec">
				{{title}}
			</view>
		</view>
		<view class="mySec_list">
			<view class="mySec_list_item" v-for="(item,index) in articleList">
				<view class="mySec_list_item_top">
					<view class="info_wrap">
						<view class="avater">
							<u-lazy-load 
							:image="$globalConstVars.apiFilesUrl+item.thumbnail"></u-lazy-load>
						</view>
						<view class="infos">
							<view class="infos_name">
								{{item.author}}
							</view>
							<view class="infos_time">
								{{item.publishDate}}
							</view>
						</view>
					</view>
					<view class="open_close" @click="changeOpen" v-if="title == '我的秘密空间'">
						<view class="open_close_icon" v-if="open">
							<u-icon :name="require('@/static/images/common/open.png')" size="25"></u-icon>
							<text>对外公开</text>
						</view>
						<view class="open_close_icon" v-else>
							<u-icon :name="require('@/static/images/common/close.png')" size="25"></u-icon>
							<text>取消公开</text>
						</view>
					</view>
					<view class="open_close" v-else>
						<view class="open_close_icon hasfocus" v-if="index == 0">
							<text>已关注</text>
						</view>
						<view class="open_close_icon nofocus" v-else>
							<text>＋关注</text>
						</view>
					</view>
				</view>
				<view class="mySec_list_item_cont">
					<view class="item_text" v-if="item.title">
						{{item.title}}
					</view>
					<view class="item_text" v-if="item.summary">
						{{item.summary}}
					</view>
					<view class="item_images">
						<view class="box-1">
							<!-- <u-lazy-load
							:image="require('@/static/images/common/test_pic.png')"></u-lazy-load> -->
						</view>
					</view>
				</view>
				<view class="save_diss_wrap">
					<view class="save_diss">
						<view class="">
							<u-icon :name="require('@/static/images/common/heart.png')" size="35"></u-icon>
							<text>24</text>
						</view>
						<view class="" @click="goPinglun(item)">
							<u-icon :name="require('@/static/images/common/diss.png')" size="35"></u-icon>
							<text>50</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			title:{
				type:String,
				default:"我的秘密空间"
			},
			showTitle:{
				type:Boolean,
				default:true
			},
			articleList:{
				type:Array,
				default:[]
			}
		},
		data(){
			return{
				arr:6
			}
		},
		methods:{
			goPinglun(item){
				// 跳转评论区
				this.$u.route('/pages/category/guanlan_detail/guanlan_detail',{item:JSON.stringify(item)});
			}
		}
	}
</script>

<style lang="scss">
	.mySec{
		width: 194rpx !important;
		margin: 40rpx 0 30rpx 0;
	}
	.mySec_list{
		width: 100%;
		padding-bottom: 20rpx;
		.mySec_list_item{
			padding: 30rpx 0;
			border-bottom: 2rpx solid #61676F;
			&_top{
				display: flex;
				justify-content: space-between;
				align-items: center;
				.info_wrap{
					flex: 1;
					display: flex;
					align-items: center;
					.avater{
						height: 90rpx;
						width: 90rpx;
						overflow: hidden;
						border-radius: 50%;
						background: #ededed;
						overflow: hidden;
						margin-right: 14rpx;
					}
					.infos{
						color: #fff;
						.infos_name{
							font-size: 27rpx;
						}
						.infos_time{
							font-size: 24rpx;
							margin-top: 16rpx;
						}
					}
				}
				.open_close{
					width: 120rpx;
					color: #02B9EE;
					font-size: 18rpx;
					display: flex;
					align-items: center;
					justify-content: flex-end;
					margin-left: 5rpx;
					.open_close_icon{
						display: flex;
						align-items: center;
						justify-content: center;
						text{
							margin-left: 7rpx;
						}
					}
					.hasfocus{
						text-align: center;
						line-height: 31rpx;
						width: 82rpx;
						height: 31rpx;
						border: 1rpx solid #02B9EE;
						border-radius: 5px;
					}
					.nofocus{
						text-align: center;
						line-height: 31rpx;
						width: 82rpx;
						height: 31rpx;
						background-color: #02B9EE;
						color: #fff;
						border-radius: 5px;
					}
				}
			}
			&_cont{
				.item_text{
					color: #fff;
					font-size: 24rpx;
					margin: 25rpx 0;
					line-height: 40rpx;
				}
				.item_images{
					display: flex;
					flex-wrap: wrap;
					width: 100%;
					overflow: hidden;
					.box-1{
						width:217rpx;
						height: 217rpx;
						margin-right: 20rpx;
						margin-bottom: 20rpx;
						background-color: #ededed;
					}
					.box-1:nth-child(3n){
						margin-right: 0;
					}
				}
			}
			.save_diss_wrap{
				display: flex;
				justify-content: flex-end;
				align-items: center;
				margin-top: 30rpx;
				.save_diss{
					display: flex;
					align-items: center;
					font-size: 26rpx;
					color: #fff;
					view{
						margin-left: 20rpx;
						display: flex;
						align-items: center;
						text{
							margin-left: 13rpx;
						}
					}
				}
			}
		}
	}
</style>
