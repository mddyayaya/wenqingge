<template>
    <!-- 页面主体 -->
    <view class="shequn">
        <!--提示框组件-->
        <u-toast ref="uToast" />
        <!--无网络组件-->
        <u-no-network></u-no-network>
        <!--头部组件-->
        <u-navbar :is-back="false" title="社群" :background="background" :title-color="titleColor" :custom-back="about"></u-navbar>
		<!-- 页面组件封装 -->
		<coreshequnPage></coreshequnPage>
		<view class="add" @click="goAddpage">
			<image src="@/static/images/common/addpaper.png" mode="widthFix"></image>
		</view>
        <!--版权组件-->
        <copyright></copyright>
        <!--返回顶部组件-->
        <u-back-top :scroll-top="scrollTop" :duration="500"></u-back-top>
        <!-- 登录提示 -->
        <coreshop-login-modal></coreshop-login-modal>
		<u-loadmore :status="loadStatus" :icon-type="loadIconType" :load-text="loadText" 
		color="#fff"
		margin-top="20" margin-bottom="20" />
    </view>
</template>
<script>
    import coreshequnPage from '@/components/coreshequn-page/coreshequn.vue';
    import copyright from '@/components/coreshop-copyright/coreshop-copyright.vue';
    import { goods } from '@/common/mixins/mixinsHelper.js';
	let that;
    export default {
        mixins: [goods],
        components: {
            copyright,
            coreshequnPage
        },
        data() {
            return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				loadStatus: 'loadmore',
				loadIconType: 'flower',
				loadText: {
				    loadmore: '轻轻上拉',
				    loading: '努力加载中',
				    nomore: '实在没有了'
				},
            };
        },
        computed: {
        },
        onLoad(e) {
			that = this;
            this.initData();
        },
        methods: {
            // 首页初始化获取数据
            initData() {
				
            },
        },
        onPullDownRefresh() {
            uni.stopPullDownRefresh();
        },
		onReachBottom() {
		    if (this.loadStatus != 'nomore') {
		        // this.getGoods();
		    }
		},
        //分享
        onShareAppMessage(res) {
            return {
                title: this.$store.state.config.shareTitle,
                imageUrl: this.$store.state.config.shareImage,
                path: this.shareUrl
            }
        },
        onShareTimeline(res) {
            return {
                title: this.$store.state.config.shareTitle,
                imageUrl: this.$store.state.config.shareImage,
                path: this.shareUrl
            }
        },
        onPageScroll: function (e) {
            this.scrollTop = e.scrollTop;
            if (e.scrollTop <= 100) {
                this.opacity = e.scrollTop / 100;
            } else if (this.scrollTop > 100) {
                this.opacity = 1;
            }
        },
    };
</script>
<style lang="scss" scoped>
    @import 'shequn.scss';
</style>