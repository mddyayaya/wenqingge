<template>
    <!-- 页面主体 -->
    <view>
        <u-toast ref="uToast" /><u-no-network></u-no-network>
        <u-navbar :custom-back="goBack" :title="title"></u-navbar>
        <view class="sigin-bg">
			<view class="sigin_top_img">
				<u-lazy-load class="top_img" :image="image"></u-lazy-load>
				<view class="share_save">
					<view class="zhanwei"></view>
					<view class="zhenshi">
						<u-icon class="save" name="star" @click="isSave=!isSave" color="#fff" size="40rpx" v-if="!isSave"></u-icon>
						<u-icon class="save" name="star-fill" @click="isSave=!isSave" size="40rpx" color="#ffd700" v-else></u-icon>
						<u-icon class="share" name="zhuanfa" size="40rpx" color="#fff"></u-icon>
					</view>
				</view>
			</view>
			<view class="sigin_wrap">
				<view class="sigin_wrap_titles u-line-1">
					<view class="titles_left">
						故宫以东·书香之旅------文沁阁
					</view>
					<view class="title_price">
						￥888
					</view>
				</view>
				<view class="items_offer">
					<view class="offer_item">
						<u-icon name="coupon" color="#000"></u-icon>
						<text>电子票</text>
					</view>
					<view class="offer_item">
						<u-icon name="checkmark-circle" color="#000"></u-icon>
						<text>需预约</text>
					</view>
				</view>
				<view class="times">
					2021-11-25 14:00-17:00
				</view>
				<view class="shuoming">
					活动市场仅供参考，具体咨询现场工作人员
				</view>
				<view class="address">
					地址：北京市东城区东直门XXXX号北京市东城区东直门XXXX号北京市东城区东直门XXXX号
				</view>
			</view>
        </view>
		<view class="sigin-bgs">
			<view class="part2_titles">
				活动详情
			</view>
			<view class="part2_list">
				<view class="part2_list_item" v-for="item in 5">
					<u-lazy-load :image="image"></u-lazy-load>
				</view>
			</view>
		</view>
		<view class="sigin-bgs sigin-bgs-noborder">
			<view class="part2_titles">
				相关产品
			</view>
			<view class="products">
				<view class="products_item" v-for="item in 2">
					<view class="top_img">
						<u-lazy-load :image="image"></u-lazy-load>
						<view class="tips">
							8888
						</view>
					</view>
					<view class="bottom_desc">
						<view class="bottom_title u-line-1">
							精美文创礼品精美文创礼品精美文创礼品
						</view>
						<view class="bottom_sub u-line-1">
							精美文创礼品精美文创礼品精美文创礼品
						</view>
						<view class="bottom_price">
							¥66
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="sigin_btn">
			<view class="sigin" v-if="!isOver" @click="goSignDetail">
				报名参加
			</view>
			<view class="sigin isover" v-else>
				已结束
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
                title: '活动详情',
				image:"",
				isSave:false,
				isOver:false
            };
        },
        onLoad(e) {
			
        },
        computed: {
			
        },
        methods: {
            goSignDetail(){
				let url = '/pages/member/sigin/sigindetail/sigindetail'
				this.$u.route(url)
			}
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
    @import "sigin.scss";
</style>