<template>
    <view class="my_integral">
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="我的特权" :background="background" :title-color="titleColor"></u-navbar>
        <!-- <u-sticky offset-top="180"> -->
            <view class="integral-top">
				<image :src="require('@/static/images/common/bg.png')" mode="widthFix"></image>
                <view class="u-font-sm coreshop-text-white  u-margin-bottom-20">
                    积分余额
                </view>
                <view class="integral-datas coreshop-text-white u-margin-bottom-20 ">
					<view class="databaseicon">
						<image :src="require('@/static/images/common/count.png')"></image>
					</view>
					<view class="database-font">
						{{pointList.length ? pointList[0].balance : 0}}
					</view>
                </view>
                <view class="u-font-sm coreshop-text-white">
                    截止{{ nowDate }}可用积分
                </view>
            </view>
        <!-- </u-sticky> -->
        <!-- 积分记录列表 -->
        <view v-if="pointList.length>0">
            <view class="coreshop-log-item u-flex u-row-between u-m-b-30" v-for="item in pointList" :key="item.id">
                <view class="item-left coreshop-flex coreshop-align-center">
                    <image class="log-img" v-if="buy" :src="require('@/static/images/common/buy_icon.png')" mode=""></image>
                    <image class="log-img" :src="require('@/static/images/common/order_icon.png')" mode=""></image>
                    <view class="coreshop-flex coreshop-flex-direction coreshop-align-start">
                        <view class="log-name coreshop-text-white">{{ item.typeName }}</view>
                        <view class="log-notice coreshop-text-grey">{{ item.remarks }}</view>
                    </view>
                </view>
                <view class="item-right coreshop-flex coreshop-flex-direction coreshop-align-end">
                    <view class="log-num coreshop-text-blue">{{ item.num > 0 ? '+' + item.num : item.num }}</view>
                    <view class="log-date coreshop-text-grey">{{ $u.timeFormat(item.createTime, 'yyyy.mm.dd') }}</view>
                </view>
            </view>
            <!-- 更多 -->
            <u-loadmore :status="status" :icon-type="iconType" :load-text="loadText" margin-top="20" margin-bottom="20" />
        </view>

        <!-- 无数据时默认显示 -->
        <view class="coreshop-emptybox" v-else>
            <u-empty :src="$globalConstVars.apiFilesUrl+'/static/images/empty/history.png'" icon-size="300" text="暂无积分记录" mode="list"></u-empty>
        </view>
    </view>
</template>
<script>
    export default {
        data() {
            return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
                page: 1,
                limit: 10,
                pointList: [], // 积分记录
                status: 'loadmore',
                iconType: 'flower',
				buy:false,
                loadText: {
                    loadmore: '轻轻上拉',
                    loading: '努力加载中',
                    nomore: '实在没有了'
                }
            }
        },
        onLoad() {
            this.userPointLog()
        },
        computed: {
            nowDate() {
                return this.$u.timeFormat(Math.round(new Date().getTime() / 1000))
            }
        },
        methods: {
            userPointLog() {
                let _this = this
                let data = {
                    page: _this.page,
                    limit: _this.limit
                }
                _this.status = 'loading'
                _this.$u.api.pointLog(data).then(res => {
                    if (res.status) {
                        _this.pointList = [..._this.pointList, ...res.data]
                        // 判断数据是否加载完毕
                        if (_this.page < res.otherData.totalPages) {
                            _this.page++
                            _this.status = 'loadmore'
                        } else {
                            _this.status = 'nomore'
                        }
                    } else {
                        // 接口請求出錯
                        _this.$u.toast(res.msg)
                        _this.status = 'loadmore'
                    }
                })
            }
        },
        // 页面滚动到底部触发事件
        onReachBottom() {
            let _this = this
            if (_this.status === 'loadmore') {
                _this.userPointLog()
            }
        }
    }
</script>
<style lang="scss">
    @import "index.scss";
</style>
