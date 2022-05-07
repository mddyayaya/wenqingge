<template>
    <!-- 页面主体 -->
    <view>
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="选择项目" :background="background" :title-color="titleColor"></u-navbar>
        <view class="sigin-bg">
			<view class="good_info">
				<view class="good_info_top">
					<view class="good_image">
						<u-image width="100%" height="160rpx" :src="require('@/static/images/seller/test.png')">
							<u-loading slot="loading"></u-loading>
						</u-image>
					</view>
					<view class="good_right">
						<view class="good_title">
							故宫以东 书香之旅——文沁阁
						</view>
						<view class="good_desc">
							文创阁艺术品店，XX系列
						</view>
					</view>
				</view>
				<view class="good_info_bottom">
					<view class="good_info_bottom_item">
						<view class="good_info_bottom_left">
							<u-icon name="clock" color="#fff" size="30"></u-icon>
							<view class="u-m-l-10">时间</view>
						</view>
						<view class="good_info_bottom_right">
							2022-2-22 10:00-15:00
						</view>
					</view>
					<view class="good_info_bottom_item">
						<view class="good_info_bottom_left">
							<u-icon name="map" color="#fff" size="30"></u-icon>
							<view class="u-m-l-10">地点</view>
						</view>
						<view class="good_info_bottom_right">
							北京市东城区东直门XX号
						</view>
					</view>
					<view class="good_info_bottom_item">
						<view class="good_info_bottom_left">
							<u-icon name="rmb-circle" color="#fff" size="30"></u-icon>
							<view class="u-m-l-10">价格</view>
						</view>
						<view class="good_info_bottom_right">
							根据选择付费
						</view>
					</view>
				</view>
			</view>
			<!-- 门票规格 -->
			<view class="good_info" @click.self="changeLevel(1)">
				<view class="good_level">
					<view class="">
						普通门票
					</view>
					<view class="" v-if="selectLevel == 1">
						<u-number-box v-model="level1Num"></u-number-box>
					</view>
					<view class="onsell" v-else>售卖中</view>
				</view>
				<view class="goods_price">
					￥168
				</view>
				<view class="good_desc_user">
					<textarea type="text" value="" v-model="level1" placeholder="备注说明"/>
				</view>
			</view>
			<view class="good_info" @click.self="changeLevel(2)">
				<view class="good_level">
					<view class="">
						VIP门票
					</view>
					<view class="" v-if="selectLevel == 2">
						<u-number-box v-model="level2Num"></u-number-box>
					</view>
					<view class="onsell" v-else>售卖中</view>
				</view>
				<view class="goods_price">
					￥288
				</view>
				<view class="good_desc_user" v-if="selectLevel == 2">
					<textarea type="text" value="" v-model="level2" placeholder="备注说明"/>
				</view>
			</view>
			<view class="good_info" @click.self="changeLevel(3)">
				<view class="good_level">
					<view class="">
						套餐门票
					</view>
					<view class="" v-if="selectLevel == 3">
						<u-number-box v-model="level3Num"></u-number-box>
					</view>
					<view class="onsell" v-else>售卖中</view>
				</view>
				<view class="goods_price">
					￥1688
				</view>
				<view class="good_desc_user" v-if="selectLevel == 3">
					<textarea type="text" value="" v-model="level3" placeholder="备注说明"/>
				</view>
			</view>
        </view>
        <!--占位底部距离-->
        <view class="coreshop-tabbar-height" />

        <!--底部操作-->
        <view class="coreshop-good-footer-fixed">
			<view class="buynow" type="success" size="medium" shape="circle" @click="nextStep()">下一步</view>
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
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
                title: '选择项目',
				level1Num:1,
				level2Num:1,
				level3Num:1,
				level1:"",
				level2:"",
				level3:"",
				selectLevel:1
            };
        },
        onLoad(e) {
			
        },
        computed: {
			
        },
        methods: {
			changeLevel(level){
				this.selectLevel = level;
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
	@import "./nextStep.scss";
</style>