<template>
    <view class="history_wrap">
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="我的足迹" :background="background" :title-color="titleColor"></u-navbar>
		<view v-if="list.length" class="goodsBox">
			<u-grid :col="2" 
			:border="false" :align="center">
				<u-grid-item bg-color="transparent"
				 :show="item.show" :index="index"
				v-for="(item,index) in list" :key="item.id" 
				@click="goGoodsDetail(item.goodsId)">
					<view class="good_box">
						<!-- 警告：微信小程序中需要hx2.8.11版本才支持在template中结合其他组件，比如下方的lazy-load组件 -->
						<view class="good_image">
							<u-lazy-load 
							height="187"
							imgMode="aspectFit"
							threshold="-150" border-radius="10" :image="item.goodImage"></u-lazy-load>
						</view>
						<view class="good_item">
							<view class="good_title u-line-2">
								{{item.goodsName}}
							</view>
							<view class="good-price">
								￥{{item.price}} 
								<!-- <span class="u-font-xs  coreshop-text-through u-margin-left-15 coreshop-text-gray">￥{{item.mktprice}}</span> -->
							</view>
						</view>
					</view>
				</u-grid-item>
			</u-grid>
		    <u-loadmore :status="status" :icon-type="iconType" :load-text="loadText" margin-top="20" margin-bottom="20" />
		</view>
		<view class="seller_list"  v-if="list.length">
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
						@click="goSigin(index == 1?false:true)">
							{{index == 1?'已结束':'立即报名'}}
						</view>
					</view>
				</view>
			</view>
		</view>
        <!-- 无数据时默认显示 -->
        <view class="coreshop-emptybox" v-else>
            <u-empty :src="$globalConstVars.apiFilesUrl+'/static/images/empty/collect.png'" icon-size="300" text="暂无足迹" mode="list"></u-empty>
        </view>
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
                page: 1,
                limit: 10,
                list: [], // 商品浏览足迹
                loadStatus: 'more',
                options: [
                    {
                        text: '收藏',
                        style: {
                            backgroundColor: '#007aff'
                        }
                    },
                    {
                        text: '删除',
                        style: {
                            backgroundColor: '#dd524d'
                        }
                    }
                ],
                status: 'loadmore',
                iconType: 'flower',
                loadText: {
                    loadmore: '轻轻上拉',
                    loading: '努力加载中',
                    nomore: '实在没有了'
                }
            };
        },
        onLoad() {
            this.goodsBrowsing()
        },
        onReachBottom() {
            if (this.status === 'loading') {
                this.goodsBrowsing();
            }
        },
        methods: {
            goodsBrowsing() {
                let data = {
                    page: this.page,
                    limit: this.limit
                }
                this.status = 'loading'
                this.$u.api.goodsBrowsing(data).then(res => {
                    if (res.status) {
                        let _list = res.data.list
                        _list.forEach(item => {
                            item.show = false;
                        })
                        this.list = [...this.list, ..._list]
                        if (res.data.count > this.list.length) {
                            this.page++
                            this.status = 'loading'
                        } else {
                            this.status = 'nomore'
                        }
                        console.log(res.data);
                    } else {
                        this.$u.toast(res.msg)
                    }
                })
            },
            click(index, index1) {
                let _this = this;
                if (index1 == 0) {
                    let data = {
                        id: _this.list[index].goodsId
                    }
                    _this.$u.api.goodsCollection(data).then(res => {
                        if (res.status) {
                            _this.$refs.uToast.show({
                                title: res.msg, type: 'success', callback: function () {
                                    _this.$nextTick(() => {
                                        _this.list[index].isCollection = !this.list[index].isCollection
                                    })
                                }
                            })
                        } else {
                            _this.$u.toast(res.msg)
                        }
                    })
                } else if (index1 == 1) {
                    let data = {
                        id: _this.list[index].goodsId
                    }
                    _this.$u.api.delGoodsBrowsing(data).then(res => {
                        if (res.status) {
                            _this.list.splice(index, 1)
                            _this.$refs.uToast.show({ title: res.msg, type: 'success' })
                        } else {
                            _this.$u.toast(res.msg)
                        }
                    })

                }
            },
            // 如果打开一个的时候，不需要关闭其他，则无需实现本方法
            open(index) {
                this.list[index].show = true;
                this.list.map((val, idx) => {
                    if (index != idx) this.list[idx].show = false;
                })
            }
        }
    };
</script>

<style lang="scss" scoped>
    @import "index.scss";
</style>