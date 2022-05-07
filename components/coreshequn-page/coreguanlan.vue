<template>
	<view class="guanlan_wrap">
		<!-- 观澜模块化组件 -->
		<coreshopGuanzhu v-if="selectType == 1"></coreshopGuanzhu>
		<coreshopBar v-if="selectType == 2"></coreshopBar>
		<coreshopDaren title="全部观澜" 
		:articleList="articleList"
		:showTitle="selectType == 1?false:true"></coreshopDaren>
        <u-back-top :scroll-top="scrollTop" :duration="300"></u-back-top>
	</view>
</template>

<script>
    import coreshopBar from './coreguanlan-daren.vue';
    import coreshopGuanzhu from './coreguanlan-guanzhu.vue';
    import coreshopDaren from './coreshequn-sec.vue';
	export default {
		props:{
			articleList:{
				type:Array,
				default:[]
			}
		},
		data() {
			return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
				selectType:1,
                scrollTop: 0,
			}
		},
		components:{
			coreshopBar,
			coreshopDaren,
			coreshopGuanzhu
		},
		methods: {
			changeType(type){
				this.selectType = type;
			},
			onPageScroll(e) {
				this.scrollTop = e.scrollTop;
			},
		},
	}
</script>

<style lang="scss">
	.guanlan_wrap{
		padding:0 30rpx;
	}
</style>
