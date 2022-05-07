<template>
    <view>
        <block v-for="(item,index) in coreshopdata" :key="index">
            <!--图片轮播（修复）-->
            <coreshopimgSlide :coreshopdata="item" v-if="item.widgetCode=='imgSlide' "></coreshopimgSlide>
            <!--公告（修复）-->
            <coreshopnotice :coreshopdata="item" v-if="item.widgetCode=='notice' "></coreshopnotice>
        </block>
		<!-- 选项卡 -->
		<coreshopChecklist :typeList="typeList" 
		@recommendAvtivityList="recommendAvtivityList"
		@initAvtivityList="initAvtivityList"></coreshopChecklist>
		<!--活动列表-->
		<coreshopgoods :recommendListData="recommendListData"></coreshopgoods>
		<!--场地-->
		<coreshoparea @nearbyStoreList="nearbyStoreList" :nearbyStoreListData="nearbyStoreListData"></coreshoparea>
    </view>
</template>

<script>
    import coreshopimgSlide from '@/components/coreshop-page/coreshop-imgSlide.vue'
    import coreshopnotice from '@/components/coreshop-page/coreshop-notice.vue'
    import coreshopChecklist from '@/components/coreshop-page/coreshop-checklist.vue'
    import coreshopgoods from '@/components/coreshop-page/coreshop-goods.vue'
    import coreshoparea from '@/components/coreshop-page/coreshop-area.vue'
	
    import coreshopsearch from '@/components/coreshop-page/coreshop-search.vue'
    import coreshopcoupon from '@/components/coreshop-page/coreshop-coupon.vue'
    import coreshopblank from '@/components/coreshop-page/coreshop-blank.vue'
    import coreshoptextarea from '@/components/coreshop-page/coreshop-textarea.vue'
    import coreshopvideo from '@/components/coreshop-page/coreshop-video.vue'
    import coreshopimgWindow from '@/components/coreshop-page/coreshop-imgWindow.vue'
    import coreshopimgSingle from '@/components/coreshop-page/coreshop-imgSingle.vue'
    import coreshopgoodTabBar from '@/components/coreshop-page/coreshop-goodTabBar.vue'
    import coreshoparticle from '@/components/coreshop-page/coreshop-article.vue'
    import coreshoparticleClassify from '@/components/coreshop-page/coreshop-articleClassify.vue'
    import coreshopnavBar from '@/components/coreshop-page/coreshop-navBar.vue'
    import coreshopgroupPurchase from '@/components/coreshop-page/coreshop-groupPurchase.vue'
    import coreshoprecord from '@/components/coreshop-page/coreshop-record.vue'
    import coreshoppinTuan from '@/components/coreshop-page/coreshop-pinTuan.vue'
    import coreshopservice from '@/components/coreshop-page/coreshop-service.vue'
    import coreshoptabbar from '@/components/coreshop-page/coreshop-tabbar.vue'
    import coreshopadpop from '@/components/coreshop-page/coreshop-adpop.vue'
    import coreshopContent from '@/components/coreshop-page/coreshop-content.vue'

    export default {
        name: 'coreshopPage',
        components: {
            coreshopimgSlide,
            coreshopsearch,
            coreshopnotice,
            coreshopcoupon,
            coreshopblank,
            coreshoptextarea,
            coreshopvideo,
            coreshopimgWindow,
            coreshopimgSingle,
            coreshopgoods,
			coreshoparea,
            coreshopgoodTabBar,
            coreshoparticle,
            coreshoparticleClassify,
            coreshopnavBar,
            coreshopgroupPurchase,
            coreshoprecord,
            coreshoppinTuan,
            coreshopservice,
            coreshoptabbar,
            coreshopadpop,
            coreshopContent,
			coreshopChecklist
        },
		methods:{
			recommendAvtivityList(){
				this.$emit("recommendAvtivityList")
			},
			initAvtivityList(typeId){
				this.$emit("initAvtivityList",typeId)
			},
			nearbyStoreList(code){
				this.$emit("nearbyStoreList",code)
			}
		},
        props: {
            coreshopdata: {
                default: function () {
                    return []
                }
            },
            recommendListData: {
                default: function () {
                    return []
                }
            },
            mainActivityData: {
                default: function () {
                    return []
                }
            },
			typeList:{
                default: function () {
                    return []
                },
			},
			nearbySlider:{
                default: function () {
                    return []
                },
			},
			nearbyData1:{
                default: function () {
                    return []
                },
			},
			nearbyData2:{
                default: function () {
                    return []
                },
			},
			nearbyData3:{
                default: function () {
                    return []
                },
			}
        }
    }
</script>
