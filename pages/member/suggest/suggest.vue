<template>
	<view class="suggest_wrap">
        <!--提示框组件-->
        <u-toast ref="uToast" />
        <!--无网络组件-->
        <u-no-network></u-no-network>
        <u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-fixed='true'
		title="反馈建议" :background="background" :title-color="titleColor"></u-navbar>
		<!-- 反馈建议 -->
		<view class="suggest">
			<view class="edit_wrap">
				<view class="content">
					<u-input type="textarea" 
					v-model="content"
					placeholder="内容编辑..."></u-input>
				</view>
			</view>
			<view class="pub_btn_wrap">
				<view class="zhanwei"></view>
				<view class="pub_btn">
					<u-icon :name="require('@/static/images/common/gl_pubicon.png')"
					size="30rpx"
					></u-icon>
					<text>反馈</text>
				</view>
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
	export default {
		data() {
			return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				content:''
			}
		},
		components:{},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	@import './suggest.scss';
</style>
