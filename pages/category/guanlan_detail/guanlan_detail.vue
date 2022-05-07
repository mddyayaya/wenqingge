<template>
	<view class="coreshequnMygroup">
		<!--提示框组件-->
		<u-toast ref="uToast" />
		<!--无网络组件-->
		<u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-fixed='true'
		title="详情" :background="background" :title-color="titleColor"></u-navbar>
		<!-- 观澜详情 -->
		<view class="mySec_list">
			<view class="mySec_list_item">
				<view class="mySec_list_item_top">
					<view class="info_wrap">
						<view class="avater">
							<u-lazy-load 
							:image="require('@/static/images/common/test_pic.png')"></u-lazy-load>
						</view>
						<view class="infos">
							<view class="infos_name">
								刘畊宏
							</view>
							<view class="infos_time">
								一小时前 · 北京
							</view>
						</view>
					</view>
					<view class="open_close">
						<view class="open_close_icon hasfocus" v-if="index == 0">
							<text>已关注</text>
						</view>
						<view class="open_close_icon nofocus" v-else>
							<text>＋关注</text>
						</view>
					</view>
				</view>
				<view class="mySec_list_item_cont">
					<view class="item_text">
						人如果没有了希望和梦想，那么跟咸鱼有什么区别？人如果心里除了工作还是工作，那人生还有什么意义？
					</view>
					<view class="item_images" v-if="index > 3">
						<view class="box-1" v-for="items in 9">
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
						<view class="" @click="goPinglun">
							<u-icon :name="require('@/static/images/common/diss.png')" size="35"></u-icon>
							<text>50</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="guanlan_diss_wrap">
			<view class="guanlan_diss">
				<view class="fav_icon">
					<u-icon name="heart" size="35" color="#fff"></u-icon>
				</view>
				<view class="fav_list">
					<view class="fav_pic" v-for="item in 10">
						<u-image height="60rpx" :src="require('@/static/images/common/test_pic.png')">
							<u-loading slot="loading"></u-loading>
						</u-image>
					</view>
				</view>
			</view>
			<view class="guanlan_diss_list" 
				:key="index"
				v-for="(item,index) in msgLists">
				<view class="first_diss">
					<view class="diss_info">
						<view class="diss_icon">
							<u-icon 
							color="#fff"
							:name="require('@/static/images/common/diss.png')" 
							size="35"></u-icon>
						</view>
						<view class="user_pic">
							<u-image 
							height="60rpx"
							width="60rpx"
							:src="item.image40">
								<u-loading slot="loading"></u-loading>
							</u-image>
						</view>
						<view class="user_info">
							<view class="user_name">
								{{item.nikeName?item.nikeName:'未知用户'}}
							</view>
							<view class="user_cont">
								{{item.content|''}}
							</view>
						</view>
					</view>
					<view class="pub_time">
						{{item.creatorTime|''}}
					</view>
				</view>
				<view class="first_diss second_diss" v-for="items in item.reply">
					<view class="diss_info">
						<view class="user_pic">
							<u-image 
							height="60rpx"
							width="60rpx"
							:src="require('@/static/images/seller/test.png')">
								<u-loading slot="loading"></u-loading>
							</u-image>
						</view>
						<view class="user_info">
							<view class="user_name">
								张天才
							</view>
							<view class="user_cont">
								神一样的对手，猪一样的队友！
							</view>
						</view>
					</view>
					<view class="pub_time">
						
					</view>
				</view>
			</view>
		</view>
		<view class="pub_diss">
			<input type="text" placeholder="发表评论" value="" v-model="pubData.content"/>
			<view class="pub_btn" @click="pubMsg">
				发表
			</view>
		</view>
		<!-- </view> -->
		<!--版权组件-->
		<copyright></copyright>
		<!--返回顶部组件-->
		<u-back-top :scroll-top="scrollTop" :duration="500"></u-back-top>
	
		<!--弹出框-->
		<!--<coreshop-modal-img :show="modalShow"  :src="$globalConstVars.apiFilesUrl+'/static/images/empty/reward.png'" @imgTap="imgTap" @closeTap="closeTap" />-->
		<!-- 登录提示 -->
		<coreshop-login-modal></coreshop-login-modal>
	</view>
</template>

<script>
	let that = this;
	export default {
		data(){
			return{
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				searchData:{
					limit:10,
					page:1,
					rootId: null,
					aid: "",
					childSize: 3
				},
				// 消息列表
				msgLists:[],
				pubData:{
					articleId: 0,
					content: "",
					userId: 0
				}
			}
		},
		onLoad({item}) {
			that = this;
			let userInfo = this.$store.state.userInfo;
			let params = JSON.parse(item);
			that.searchData.aid = params.id;
			// that.searchData.aid = 8;
			that.pubData.articleId = params.id;
			that.pubData.userId = userInfo.id;
			that.getdata();
		},
		mounted() {
		},
		methods:{
			getdata(){
				that.$u.api.msgList(that.searchData).then(res => {
					if (res.code == 0) {
						that.msgLists = res.data;
					}
				});
			},
			// 发布留言
			pubMsg(){
				if(that.pubData.content != ""){
					that.$u.api.pubMsg(that.pubData).then(res => {
						if (res.data.res) {
							that.pubData.content = "";
							that.getdata();
						}
					});
				}else{
					that.$refs.uToast.show({
						title:"请输入留言后发布",
						type:"default"
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	@import './guanlan_detail.scss';
</style>
