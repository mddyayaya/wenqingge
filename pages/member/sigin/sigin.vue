<template>
    <view>
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="活动详情" :background="background" :title-color="titleColor"></u-navbar>
        <!--幻灯片-->
        <view class="banner-swiper-box">
            <swiper class="banner-swiper-box" circular autoplay @change="bannerSwiper">
                <swiper-item v-for="(item,index) in goodsInfo.album" :key="index" @click="clickImg(item)">
                    <u-image width="100%" height="362rpx" :src="item">
                        <u-loading slot="loading"></u-loading>
                    </u-image>
                </swiper-item>
            </swiper>
			<!-- 分享 -->
			<view class="save_share">
				<view class="save_share_item">
					<u-icon :name="[isfav?'star-fill':'star']" size="40" :color="isfav?'#fff':'#02B9EE'"></u-icon>
				</view>
				<view class="save_share_item">
					<u-icon name="share" @click="goShare()" size="40" color="#fff"></u-icon>
				</view>
			</view>
        </view>
		<!-- 商品详情 -->
		<view class="good_detail_box">
			<view class="good_info_top">
				<view class="good_title u-line-2">
					<text>{{ product.name || '' }}</text>
				</view>
				<view class="goods_price">
					￥{{ product.price || '0.00' }}
				</view>
			</view>
			<view class="ticket_wrap">
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
			<view class="ticket_wrap_time">
				2022-2-22 10:00-15:00
			</view>
			<view class="ticket_wrap_tishi">
				活动现场仅供参考，具体请咨询现场工作人员
			</view>
			<view class="ticket_wrap_adress">
				地址：北京市东城区东直门XX号
			</view>
		</view>
        <!--内容区-->
        <view class="u-margin-left-20 u-margin-right-20 content_bg">
			<view class="titleBg u-margin-left-30 u-margin-top-30 u-margin-bottom-30">
				活动详情
			</view>
            <!--参数-->
            <view class="u-padding-bottom-10 u-padding-left-10 u-padding-right-10 coreshop-flex-direction-row" v-for="(item, index) in goodsParams" :key="index">
                <text class="coreshop-text-gray">{{ item.name || ''}}：</text>
                <text class="coreshop-text-black">{{ item.value || ''}}</text>
            </view>
            <view class="u-padding-top-10">
                <u-parse :html="goodsInfo.intro" :selectable="true" v-if="goodsInfo.intro"></u-parse>
                <!-- 无数据时默认显示 -->
                <view class="coreshop-emptybox" v-else>
                    <u-empty :src="$globalConstVars.apiFilesUrl+'/static/images/empty/data.png'" icon-size="300" text="详情为空" mode="list"></u-empty>
                </view>
            </view>
        </view>

        <!-- 分享弹窗 -->
        <view class="u-padding-10">
            <u-popup  mode="bottom" v-model="shareBox" ref="share">
                <shareByWx :goodsId="goodsInfo.id" :shareImg="goodsInfo.image" :shareTitle="goodsInfo.name" :shareContent="goodsInfo.brief" :shareHref="shareHref" @close="closeShare()"></shareByWx>
            </u-popup>
            <div id="qrCode" ref="qrCodeDiv"></div>
        </view>
		<view class="titleBg u-margin-left-30 u-margin-top-30 u-margin-bottom-30">
			相关产品
		</view>
         <!--推荐列表-->
        <view class="coreshop-goods-group" v-if="otherRecommendData.length>0">
            <u-grid :col="2" :border="false" :align="center">
                <u-grid-item bg-color="transparent" :custom-style="{padding: '0rpx'}" v-for="(item, index) in otherRecommendData" :key="index" @click="goGoodsDetail(item.id)">
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
								￥{{item.price}} <span class="u-font-xs  coreshop-text-through u-margin-left-15 coreshop-text-gray">￥{{item.mktprice}}</span>
							</view>
						</view>
					</view>
                </u-grid-item>
            </u-grid>
        </view>

        <!--占位底部距离-->
        <view class="coreshop-tabbar-height" />

        <!--底部操作-->
        <view class="coreshop-good-footer-fixed">
			<view class="buynow" type="success" size="medium" shape="circle" @click="clickDate()">报名参加</view>
        </view>
        <!-- 右边浮动球 -->
        <coreshop-fab horizontal="right" vertical="bottom" direction="vertical"></coreshop-fab>
        <!-- 登录提示 -->
        <coreshop-login-modal></coreshop-login-modal>
    </view>

</template>
<script>
    import { mapMutations, mapActions, mapState } from 'vuex';
    import { goods} from '@/common/mixins/mixinsHelper.js'
    import shareByWx from '@/components/coreshop-share/shareByWx.vue'
    export default {
        mixins: [goods],
        components: {
            shareByWx
        },
        data() {
            return {
                background: this.$coreTheme.mainNabBar.background,
                titleColor: this.$coreTheme.mainNabBar.titleColor,
                bannerCur: 0,
                current: 0, // init tab位
                goodsId: 0, // 商品id
                goodsInfo: {}, // 商品详情
                cartNums: 0, // 购物车数量
                product: {}, // 货品详情
                shopRecommendData: [], // 本店推荐数据
                otherRecommendData: [], // 其他数据
                goodsParams: [], // 商品参数信息
                type: 2, // 1加入购物车 2购买
                cartType: 1,
                isfav: false, // 商品是否收藏
                submitStatus: false,
                bottomModal: false,
                modalTitle: '',
                modalType: 'promotion',
                selectType: '',
                shareUrl: '/pages/share/jump/jump',
                shareBox: false,
                serviceDescription: {
                    commonQuestion: [],
                    delivery: [],
                    service: [],
                }
            }
        },
        onLoad(options) {
            //获取商品ID
            if (options.id != '') {
                this.goodsId = options.id;
            }
            if (this.goodsId) {
                this.getServiceDescription();
                this.getGoodsDetail();
                this.getGoodsParams();
            } else {
                this.$refs.uToast.show({ title: '获取失败', type: 'error', back: true });
            }
            //获取推荐商品数据
            this.getGoodsRecommendList();

            var _this = this
            if (this.$db.get('userToken')) {
                this.$u.api.userInfo().then(res => {
                    if (res.status) {
                        _this.userInfo = res.data
                        _this.hasLogin = true;
                        // #ifdef MP-WEIXIN
                        //微信小程序打开客服时，传递用户信息
                        var kefupara = {}
                        kefupara.nickName = res.data.nickname
                        kefupara.tel = res.data.mobile
                        _this.kefupara = JSON.stringify(kefupara)
                        // #endif
                    }
                })
            };
        },
        onShow() {
            this.submitStatus = false;
        },
        computed: {
            ...mapState({
                hasLogin: state => state.hasLogin,
                userInfo: state => state.userInfo,
            }),
            hasLogin: {
                get() {
                    return this.$store.state.hasLogin;
                },
                set(val) {
                    this.$store.commit('hasLogin', val);
                }
            },
            userInfo: {
                get() {
                    return this.$store.state.userInfo;
                },
                set(val) {
                    this.$store.commit('userInfo', val);
                }
            },
            shopName() {
                return this.$store.state.config.shopName;
            },
            shareTitle() {
                return this.$store.state.config.shareTitle;
            },
            shopLogo() {
                return this.$store.state.config.shopLogo;
            },
            // 获取店铺联系人手机号
            shopMobile() {
                return this.$store.state.config.shopMobile || 0;
            },
            shareHref() {
                let pages = getCurrentPages()
                let page = pages[pages.length - 1]
                return this.$globalConstVars.apiBaseUrl + 'wap/' + page.route + '?id=' + this.goodsId;
            }
        },
        methods: {
            bannerSwiper(e) {
                this.bannerCur = e.detail.current;
            },
            getServiceDescription() {
                let _this = this;
                this.$u.api.getServiceDescription().then(res => {
                    if (res.status == true) {
                        _this.serviceDescription.commonQuestion = res.data.commonQuestion;
                        _this.serviceDescription.delivery = res.data.delivery;
                        _this.serviceDescription.service = res.data.service;

                    } else {
                        _this.$refs.uToast.show({ title: res.msg, type: 'error', back: true })
                    }
                })
            },
			// 店家详情
			goShop(){
				this.$u.route('/pages/member/seller/seller?shopName='+this.shopName+'&isfav='+this.isfav)
			},
            // 获取商品详情
            getGoodsDetail() {
                let _this = this;
                let data = {
                    id: parseInt(this.goodsId)
                }
                // 如果用户已经登录 要传用户token
                let userToken = this.$db.get("userToken");
                if (userToken) {
                    this.$u.api.goodsDetailByToken(data).then(res => {
                        if (res.status == true) {
                            let info = res.data;
                            let products = res.data.product;
                            _this.goodsInfo = info;
                            _this.isfav = res.data.isFav;
                            _this.product = _this.spesClassHandle(products);

                            _this.buyNum = _this.product.stock >= _this.minBuyNum ? _this.minBuyNum : 0;
                            // 判断如果登录用户添加商品浏览足迹
                            if (userToken) {
                                _this.goodsBrowsing();
                            }
                        } else {
                            _this.$refs.uToast.show({ title: res.msg, type: 'error', back: true })
                        }
                    })
                } else {
                    this.$u.api.goodsDetail(data).then(res => {
                        if (res.status == true) {
                            let info = res.data;
                            let products = res.data.product;
                            _this.goodsInfo = info;
                            _this.isfav = res.data.isFav;
                            _this.product = _this.spesClassHandle(products);

                            _this.buyNum = _this.product.stock >= _this.minBuyNum ? _this.minBuyNum : 0;
                            // 判断如果登录用户添加商品浏览足迹
                            if (userToken) {
                                _this.goodsBrowsing();
                            }
                        } else {
                            _this.$refs.uToast.show({ title: res.msg, type: 'error', back: true })
                        }
                    })
                }

            },
            // 获取推荐商品信息
            getGoodsRecommendList() {
                let _this = this;
                let recommenddata = {
                    id: 10,
                    data: true
                }
                _this.$u.api.getGoodsRecommendList(recommenddata).then(res => {
                    if (res.status) {
                        _this.shopRecommendData = _this.$u.randomArray(res.data);
                    } else {
                        _this.$u.toast(res.msg)
                    }
                });

                let data = {
                    id: 10
                }
                _this.$u.api.getGoodsRecommendList(data).then(res => {
                    if (res.status) {
                        _this.otherRecommendData = _this.$u.randomArray(res.data);
                    } else {
                        _this.$u.toast(res.msg)
                    }
                });
            },
            // 多规格样式统一处理
            spesClassHandle(products) {
                // 判断是否是多规格 (是否有默认规格)
                if (products.hasOwnProperty('defaultSpecificationDescription')) {
                    let spes = products.defaultSpecificationDescription;
                    for (let key in spes) {
                        for (let i in spes[key]) {
                            if (spes[key][i].hasOwnProperty('isDefault') && spes[key][i].isDefault === true) {
                                this.$set(spes[key][i], 'cla', 'selected');
                            } else if (spes[key][i].hasOwnProperty('productId') && spes[key][i].productId) {
                                this.$set(spes[key][i], 'cla', 'not-selected');
                            } else {
                                this.$set(spes[key][i], 'cla', 'none');
                            }
                        }
                    }
                    spes = JSON.stringify(spes)
                    products.defaultSpecificationDescription = spes;
                }
                return products;
            },
            // 商品收藏/取消
            collection() {
                let data = {
                    id: this.goodsInfo.id
                }
                this.$u.api.goodsCollection(data).then(res => {
                    if (res.status) {
                        this.isfav = !this.isfav;
                        this.$refs.uToast.show({ title: res.msg, type: 'success', back: false });
                    } else {
                        this.$u.toast(res.msg);
                    }
                })
            },
            // tab点击切换
            onClickItem(index) {
                if (this.current !== index) {
                    this.current = index;
                }
            },
            // 获取商品参数信息
            getGoodsParams() {
                this.$u.api.goodsParams({
                    id: this.goodsId
                }).then(res => {
                    if (res.status == true) {
                        this.goodsParams = res.data;
                    }
                })
            },
            // 添加商品浏览足迹
            goodsBrowsing() {
                let data = {
                    id: this.goodsInfo.id
                }
                this.$u.api.addGoodsBrowsing(data).then(res => { });
            },
            // 立即购买
            buyNow() {
                if (this.buyNum > 0) {
                    let data = {
                        productId: this.product.id,
                        nums: this.buyNum,
                        type: this.type, // 区分加入购物车和购买
                        cartType: this.cartType // 区分加入购物车和购买
                    }
                    this.$u.api.addCart(data).then(res => {
                        this.submitStatus = false;
                        if (res.status) {
                            this.hideModal();
                            let cartIds = res.data;
                            this.$u.route('/pages/placeOrder/index/index?cartIds=' + JSON.stringify(cartIds));
                        } else {
                            this.$u.toast(res.msg);
                        }
                    });
                }
            },
            // 跳转到h5分享页面
            goShare() {
                this.shareBox = true;
            },
            closeShare() {
                this.shareBox = false;
            },
            // 图片点击放大
            clickImg(imgs) {
                // 预览图片
                uni.previewImage({
                    urls: imgs.split()
                });
            },
            //获取分享URL
            getShareUrl() {
                let data = {
                    client: 2,
                    url: "/pages/share/jump/jump",
                    type: 1,
                    page: 2,
                    params: {
                        goodsId: this.goodsInfo.id,
                    }
                };
                let userToken = this.$db.get('userToken');
                if (userToken && userToken != '') {
                    data['token'] = userToken;
                }
                this.$u.api.share(data).then(res => {
                    this.shareUrl = res.data
                });
            },
        },
        watch: {
            goodsInfo: {
                handler() {
                    this.getShareUrl();
                },
                deep: true
            }
        },
        //分享
        onShareAppMessage(res) {
            return {
                title: this.goodsInfo.name,
                imageUrl: this.goodsInfo.image,
                path: this.shareUrl
            }
        },
        onShareTimeline(res) {
            return {
                title: this.goodsInfo.name,
                imageUrl: this.goodsInfo.image,
                path: this.shareUrl
            }
        },
    }
</script>
<style lang="scss">
	.banner-swiper-box{
		position: relative;
		height: 362rpx;
		width: 100%;
		.save_share{
			top: 8rpx;
			right: 24rpx;
			display: flex;
			align-items: center;
			position: absolute;
			.save_share_item{
				border-radius: 50%;
				display: flex; 
				align-items: center;
				justify-content: center;
				width: 60rpx;
				height: 60rpx;	
				background: rgba($color: #fff, $alpha: 0.16);
			}
			.save_share_item:nth-child(1){
				margin-right: 20rpx;
			}
		}
	}
	.good_detail_box{
		margin: 20rpx;
		padding: 20rpx;
		background: #414E5A;
		box-shadow: 0px 30rpx 50rpx 0px rgba(33, 39, 46, 0.08);
		border-radius: 16rpx;
		font-size: 24rpx;
		color: #FFFFFF;
		&>view{
			margin-bottom: 26rpx;
		}
		.good_info_top{
			display: flex;
			justify-content: space-between;
			align-items: flex-start;
			.good_title{
				color: #fff;
				font-size: 26rpx;
				margin-right: 26rpx;
			}
			.goods_price{
				color: #02B9EE;
				font-size: 30rpx;
				font-weight: bold;
			}
		}
		.ticket_wrap{
			color: #fff;
			display: flex;
			align-items: center;
			.ticket_wrap_item{
				display: flex;
				align-items: center;
				margin-right: 37rpx;
				text{
					margin-left: 16rpx;
				}
			}
		}
		.ticket_wrap_adress{
			padding-top: 26rpx;
			border-top: 2rpx solid #61676F;
		}
		.content_bg{
			background: #414E5A;
			box-shadow: 0rx 30rpx 50rpx 0px rgba(33, 39, 46, 0.08);
			border-radius: 16rpx;
		}
	}
</style>
