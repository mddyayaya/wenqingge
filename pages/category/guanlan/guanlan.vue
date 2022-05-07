<template>
	<view>
        <!--提示框组件-->
        <u-toast ref="uToast" />
        <!--无网络组件-->
        <u-no-network></u-no-network>
        <u-navbar :is-back="false" title="观澜" :background="background" :title-color="titleColor"></u-navbar>
		<!-- 观澜模块化组件 -->
		<view class="guanlan_type">
			<view class="guanlan_type_item" 
			@click="changeType(0)"
			:class="{selectType:searchData.type==0}">
				关注
			</view>
			<view class="guanlan_type_item"
			@click="changeType(1)"
			:class="{selectType:searchData.type==1}">
				最新
			</view>
		</view>
		<coreguanlan :articleList="articleList"></coreguanlan>
		<!-- </view> -->
        <!--版权组件-->
        <copyright></copyright>
        <!--返回顶部组件-->
        <u-back-top :scroll-top="scrollTop" :duration="500"></u-back-top>
		<u-loadmore :status="loadStatus" :icon-type="loadIconType" :load-text="loadText" margin-top="20" margin-bottom="20" />
        <!--弹出框-->
        <!--<coreshop-modal-img :show="modalShow"  :src="$globalConstVars.apiFilesUrl+'/static/images/empty/reward.png'" @imgTap="imgTap" @closeTap="closeTap" />-->
        <!-- 登录提示 -->
        <coreshop-login-modal></coreshop-login-modal>
	</view>
</template>

<script>
    import coreguanlan from '@/components/coreshequn-page/coreguanlan.vue';
	let that;
	export default {
		data() {
			return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				searchData:{
					page: 1,
					limit: 10,
					keywords: "",
					id: 0,
					type: 0,
					sort: ""
				},
                loadStatus: 'loadmore',
                loadIconType: 'flower',
                loadText: {
                    loadmore: '轻轻上拉',
                    loading: '努力加载中',
                    nomore: '实在没有了'
                },
				articleList:[],
				totalPages:0
			}
		},
		components:{coreguanlan},
		mounted() {
			that.listData();
		},
		onLoad() {that = this;},
		methods: {
			// 列表接口
			listData(){
				that.$u.api.guanlanArticleList(that.searchData).then(res => {
					if (res.code == 0) {
						that.articleList = that.articleList.concat(res.data);
						if (res.totalPages > that.searchData.page) {
						    that.loadStatus = 'loadmore';
						    that.searchData.page++;
						} else {
						    // 数据已加载完毕
						    that.loadStatus = 'nomore';
						}
					}
				});
			},
			// 切换最新，关注
			changeType(type){
				that.loadStatus = 'loadmore';
				that.searchData.type = type;
				that.articleList = [];
				that.page = 1;
				that.listData();
			}
		},
		// 下拉加载
		onReachBottom() {
			if (that.loadStatus != 'nomore') {
				that.listData();
			}
		}
	}
</script>

<style lang="scss">
	.guanlan_type{
		display: flex;
		align-items: center;
		justify-content: center;
		height: 70rpx;
		border-bottom: 1rpx solid #2B323B;
		.guanlan_type_item{
			margin-right: 58rpx;
			color: #fff;
			line-height: 60rpx;
			font-size: 24rpx;
			border-bottom: 2rpx solid rgba($color: #000000, $alpha: 0);
		}
		.selectType{
			border-bottom: 2rpx solid #02B9EE;
		}
	}
</style>
