<template>
	<view>
		<u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="文沁阁艺术品商店" :background="background" :title-color="titleColor"></u-navbar>
		<!-- 店铺信息 -->
		<view class="seller_info">
			<swiper class="seller_swiper" :indicator-dots="false"
			 :autoplay="false" :interval="3000" :duration="1000">
				<swiper-item class="seller_swiper">
					<u-image width="100%" height="362rpx" :src="require('@/static/images/seller/test.png')">
					    <u-loading slot="loading"></u-loading>
					</u-image>
				</swiper-item>
			</swiper>
			<view class="seller_info_des">
				<view class="seller_info_desc">
					<view class="seller_info_desc_name">
						{{shopName | ''}}
					</view>
					<view class="seller_info_desc_like">
						<u-icon :name="isfav=='true'?'heart-fill':'heart'"
						:color="isfav=='true'?'#ff0000':'#fff'" size="40"></u-icon>
					</view>
				</view>
				<view class="seller_info_time">
					<view class="">营业时间：周一至周日 9:00-18:00</view>
					<view class="">地址：东城区西直门xx号</view>
					<view class="">咨询电话：010-98000000</view>
				</view>
				<view class="seller_info_cont">
					店铺简介：位于东直门内北小街的文沁阁的书店于2020年9月开业，藏书3万余册，面积1000余平米，图书区、茶歇区、咖啡区、活动区、文创区装饰雅致，书香氛围浓郁，因为组织了各种丰富多彩的读书活动，吸引了大量读者参与，获得过“北京特色书店”的殊荣。 
				</view>
			</view>
			<!-- 商品和活动 -->
			<view class="sellers_active">
				<view class="seller_bar">
					<view @click="selectId = 1" :class="{seller_select:selectId==1}">店铺商品</view>
					<view @click="selectId = 2" :class="{seller_select:selectId==2}">店铺活动</view>
				</view>
				<view class="seller_list">
					<!-- 店铺商品 -->
					<view class="coreshop-goods-group" v-if="selectId == 1">
					    <u-grid :col="2" :border="false" :align="center">
					        <u-grid-item bg-color="transparent" :custom-style="{padding: '0rpx'}" v-for="(item, index) in 4" :key="index" @click="goGoodsDetail(item.id)">
								<view class="good_box">
									<!-- 警告：微信小程序中需要hx2.8.11版本才支持在template中结合其他组件，比如下方的lazy-load组件 -->
									<view class="good_image">
										<u-lazy-load 
										height="187"
										imgMode="aspectFit"
										threshold="-150" border-radius="10" :image="require('@/static/images/seller/test.png')" :index="index"></u-lazy-load>
									</view>
									<view class="good_item">
										<view class="good_title u-line-2">
											精美文创礼品精美文创文沁阁艺术品商店
										</view>
										<view class="good-price">
											￥288 <span class="u-font-xs  coreshop-text-through u-margin-left-15 coreshop-text-gray">￥398</span>
										</view>
									</view>
									<!-- <view class="good-tag-recommend" v-if="item.isRecommend">
									    推荐
									</view>
									<view class="good-tag-hot" v-if="item.isHot">
									    热门
									</view> -->
								</view>
					        </u-grid-item>
					    </u-grid>
					</view>
					<view class="active_list" v-else>
						<view class="active_list_item" v-for="(item,index) in 4">
							<view class="left_image">
								<u-lazy-load :image="require('@/static/images/seller/test.png')"></u-lazy-load>
							</view>
							<view class="right_cont">
								<view class="active_name u-line-2">
									文沁阁——图书参观活动
								</view>
								<view class="active_time">
									2022.2.22-2022.2.23
								</view>
								<view class="active_name u-line-1">
									北京市东城区东直门北小街2号
								</view>
								<view class="active_btn" 
								:class="{active_over:index == 1}"
								@click="goSigin(index == 1?false:true)">
									{{index == 1?'已结束':'立即报名'}}
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 登录提示 -->
		<coreshop-login-modal></coreshop-login-modal>
	</view>
</template>

<script>
    import { goods} from '@/common/mixins/mixinsHelper.js'
	export default {
		mixins:[goods],
		data() {
			return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				shopName:'',
				isfav:false,
				selectId:1
			}
		},
		onLoad(options) {
			this.shopName = options.shopName;
			this.isfav = options.isfav
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
	@import './seller.scss';

</style>
