<template>
    <!-- 页面主体 -->
    <view>
        <u-toast ref="uToast" /><u-no-network></u-no-network>
        <u-navbar :custom-back="goBack" :title="title"></u-navbar>
        <view class="sigin-bg">
			<view class="">
				
			</view>
        </view>
        <!-- 登录提示 -->
        <coreshop-login-modal></coreshop-login-modal>
    </view>
</template>

<script>
    import { tools } from '@/common/mixins/mixinsHelper.js';
    export default {
        mixins: [tools],
        data() {
            return {
                title: '选择项目',
            };
        },
        onLoad(e) {
			
        },
        computed: {
			
        },
        methods: {
			
        },
        watch: {
			
        },
        //分享
        onShareAppMessage(res) {
            return {
                title: this.info.title,
                path: this.shareUrl
            }
        },
        onShareTimeline(res) {
            return {
                title: this.info.title,
                path: this.shareUrl
            }
        },
    };
</script>

<style lang="scss" scoped>
    @import "sigindetail.scss";
</style>