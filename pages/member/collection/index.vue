<template>
    <view class="collection_wrap">
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="我的收藏" :background="background" :title-color="titleColor"></u-navbar>
		<view class="search_bar search_bar_coll">
			<u-search  
				:show-action="false" 
				shape="round" v-model="searchKey"
				placeholder="搜索商品"
				color="#fff"
				height="71"
				@custom="goSearch" 
				@search="goSearch"
				bgColor="#61676F"></u-search>
		</view>
		<u-subsection :list="tabs" :current="current" :animation="true" @change="onClickItem" active-color="#fff" mode="button"></u-subsection>
        <view v-if="list.length" class="goodsBox">
            <!-- <u-swipe-action :show="item.show" :index="index" v-for="(item, index) in list" :key="item.id" @click="collect" @open="open" :options="options">
                <view class="u-padding-20 u-padding-bottom-0 u-border-bottom coreshop-display-flex" @click="goGoodsDetail(item.goodsId)">
                    <u-avatar :src="item.goods.image" mode="square" size="120" class="u-margin-10"></u-avatar>
                    <view class="title-wrap">
                        <text class="u-text-left title u-line-2">{{ item.goods.name }}</text>
                        <view class="u-margin-top-10">
                            <u-icon name="clock" class="u-margin-right-6"></u-icon><text class="u-text-left desc u-line-1">{{ item.createTime }}</text>
                        </view>
                    </view>
                </view>
            </u-swipe-action> -->
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
							threshold="-150" border-radius="10" :image="item.goods.image"></u-lazy-load>
						</view>
						<view class="good_item">
							<view class="good_title u-line-2">
								{{item.goods.name}}
							</view>
							<view class="good-price">
								￥{{item.goods.price}} 
								<span class="u-font-xs  coreshop-text-through u-margin-left-15 coreshop-text-gray">￥{{item.goods.mktprice}}</span>
							</view>
						</view>
					</view>
				</u-grid-item>
			</u-grid>
            <u-loadmore :status="status" :icon-type="iconType" :load-text="loadText" margin-top="20" margin-bottom="20" />
        </view>
        <!-- 无数据时默认显示 -->
        <view class="coreshop-emptybox" v-else>
            <u-empty :src="$globalConstVars.apiFilesUrl+'/static/images/empty/collect.png'" icon-size="300" text="暂无收藏" mode="list"></u-empty>
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
                disabled: false,
                btnWidth: 180,
                show: false,
				current:0,
                tabs: ['商品', '活动', '店铺'],
                options: [
                    {
                        text: '取消',
                        style: {
                            backgroundColor: '#dd524d'
                        }
                    }
                ],
				searchKey:"",
                page: 1,
                limit: 10,
                list: [], // 商品浏览足迹
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
            this.goodsCollectionList()
        },
        onReachBottom() {
            if (this.status === 'loadmore') {
                this.goodsCollectionList()
            }
        },
        methods: {
            goodsCollectionList() {
                let data = {
                    page: this.page,
                    limit: this.limit
                }
                this.status = 'loading '
                this.$u.api.goodsCollectionList(data).then(res => {
                    if (res.status) {
                        let _list = res.data.list;
                        _list.forEach(item => {
                            item.show = false;
                        })
                        this.list = [...this.list, ..._list]

                        if (res.data.count > this.list.length) {
                            this.page++
                            this.status = 'loadmore'
                        } else {
                            this.status = 'nomore'
                        }
                    } else {
                        this.$u.toast(res.msg)
                    }
                })
            },
            // 取消收藏
            collect(index, index1) {
                let _that = this;
                let data = {
                    id: _that.list[index].goodsId
                }
                this.$u.api.goodsCollection(data).then(res => {
                    if (res.status) {
                        _that.$refs.uToast.show({
                            title: res.msg, type: 'success', callback: function () {
                                _that.$nextTick(() => {
                                    _that.list.splice(index, 1)
                                })
                            }
                        })
                    } else {
                        this.$u.toast(res.msg)
                    }
                })
            },
            // 如果打开一个的时候，不需要关闭其他，则无需实现本方法
            open(index) {
                // 先将正在被操作的swipeAction标记为打开状态，否则由于props的特性限制，
                // 原本为'false'，再次设置为'false'会无效
                this.list[index].show = true;
                this.list.map((val, idx) => {
                    if (index != idx) this.list[idx].show = false;
                })
            },
			onClickItem(current){
				this.current = current
			}
        }
    };
</script>


<style lang="scss" scoped>
    @import "index.scss";
</style>