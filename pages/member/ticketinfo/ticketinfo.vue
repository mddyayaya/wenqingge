<template>
	<view class="ticket_wrap">
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="票券详情" :background="background" :title-color="titleColor"></u-navbar>
		<view class="ticket_info">
			<image class="ticket_bg" :src="require('@/static/images/common/ticket_bg.png')" mode=""></image>
			<view class="ticket_box ticket_name">
				{{ticketInfo.name}}
			</view>
			<view class="ticket_box ticket_appointment">
				<view class="ticket_wrap_item">
					<u-icon :name="require('@/static/images/good/ticket.png')"
					size="30"
					></u-icon>
					<text>电子票</text>
				</view>
				<view class="ticket_wrap_item">
					<u-icon :name="require('@/static/images/good/date.png')"
					size="30"
					></u-icon>
					<text>需预约</text>
				</view>
			</view>
			<view class="ticket_box ticket_time">
				{{ticketInfo.time}}
			</view>
			<view class="ticket_box ticket_set">
				活动现场仅供参考，具体请咨询现场工作人员
			</view>
			<view class="ticket_box ticket_time">
				{{ticketInfo.address}}
			</view>
			<view class="ticket_code">
				<view class="ticket_code_img">
					
				</view>
				<view class="ticket_code_number">
					票号：{{ticketInfo.number}}
				</view>
			</view>
			<view class="ticket_owner">
				<u-grid :col="2" :border="false" :align="center">
					<u-grid-item bg-color="transparent" :custom-style="{padding: '0rpx'}" 
						v-for="item in ownerInfo">
						<view class="ticket_owner_info">
							{{item.label}}：{{item.value}}
						</view>
					</u-grid-item>
				</u-grid>
			</view>
		</view>
		<view class="titleBg u-margin-left-30 u-margin-top-30 u-margin-bottom-30">
			相关产品
		</view>
		 <!--推荐列表-->
		<view class="coreshop-goods-group">
		    <u-grid :col="2" :border="false" :align="center">
		        <u-grid-item bg-color="transparent" :custom-style="{padding: '0rpx'}" v-for="(item, index) in 2" :key="index" @click="goGoodsDetail(item.id)">
					<view class="good_box">
						<!-- 警告：微信小程序中需要hx2.8.11版本才支持在template中结合其他组件，比如下方的lazy-load组件 -->
						<view class="good_image">
							<u-lazy-load 
							height="187"
							imgMode="aspectFit"
							threshold="-150" border-radius="10" :image="item.image" :index="index"></u-lazy-load>
						</view>
						<view class="good_item">
							<view class="good_title u-line-2">
								{{item.name}}
							</view>
							<view class="good-price">
								￥{{item.price}}
								 <!-- <span class="u-font-xs  coreshop-text-through u-margin-left-15 coreshop-text-gray">￥{{item.mktprice}}</span> -->
							</view>
						</view>
					</view>
		        </u-grid-item>
		    </u-grid>
		</view>
		<!-- 登录提示 -->
		<coreshop-login-modal></coreshop-login-modal>
	</view>
</template>

<script>
    import { goods } from '@/common/mixins/mixinsHelper.js'
	export default {
        mixins: [goods],
		data() {
			return {
				background: this.$coreTheme.mainNabBar.background,
				titleColor: this.$coreTheme.mainNabBar.titleColor,
				ticketInfo:{
					name:"故宫以东 书香之旅——文沁阁",
					time:"2022-2-22 10:00-15:00",
					address:"北京市东城区东直门XX号",
					number:"341354235"
				},
				ownerInfo:[
					{label:"姓名",value:"张倩"},
					{label:"票种",value:"VIP门票"},
					{label:"电话",value:"18855556652"},
					{label:"票价",value:"￥85"},
					{label:"状态说明",value:"票据有效"},
					{label:"实付",value:"￥85"},
					{label:"票务说明",value:"由工作人员验票有效"},
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	@import './ticketinfo.scss';
</style>
