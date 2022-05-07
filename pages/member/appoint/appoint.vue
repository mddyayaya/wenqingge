<template>
	<view class="appoint_wrap">
		<u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="活动预约" :background="background" :title-color="titleColor"></u-navbar>
		<u-subsection :list="tabs" :current="current" :animation="true" @change="onClickItem" active-color="#fff" mode="button"></u-subsection>
		<view class="seller_list">
			<view class="active_list">
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
						@click="getTicket(index == 1?false:true)">
							{{index == 1?'已使用':'取电子票'}}
						</view>
					</view>
				</view>
			</view>
		</view>
		<u-loadmore :status="status" :icon-type="iconType" :load-text="loadText" margin-top="20" margin-bottom="20" />
		<!-- 无数据时默认显示 -->
		<view class="coreshop-emptybox">
		    <u-empty :src="$globalConstVars.apiFilesUrl+'/static/images/empty/collect.png'" icon-size="300" text="暂无活动" mode="list"></u-empty>
		</view>
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
				current:0,
                tabs: ['未使用(2)', '已使用(0)'],
                status: 'loadmore',
                iconType: 'flower',
                loadText: {
                    loadmore: '轻轻上拉',
                    loading: '努力加载中',
                    nomore: '实在没有了'
                }
			}
		},
		methods: {
			onClickItem(current){
				this.current = current;
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import './appoint.scss';
</style>
