<template>
    <view class="search_wrap">
        <u-toast ref="uToast" /><u-no-network></u-no-network>
		<u-navbar
		color="#fff"
		back-icon-color="#fff"
		:is-back="true"
		:is-fixed="true"
		title="搜索" :background="background" :title-color="titleColor"></u-navbar>
        <view class="search_bar">
			<u-search  
				:show-action="true" 
				action-text="搜索"
				:actionStyle="{'color':'#fff'}"
				shape="round" 
				:animation="false"
				v-model="key"
				placeholder="请输入关键字搜索"
				color="#fff"
				height="71"
				@search="search" @custom="search"
				bgColor="#61676F"></u-search>
        </view>

        <!--搜索区域-->
        <view class="" v-if="!deleteView">
            <!--搜索记录-->
            <view class="u-m-t-40" v-show="keys.length > 0">
                <u-section title="历史搜索" color="#fff" sub-title="删除" subColor="#fff" :arrow="false" @click="deleteKey"></u-section>
				<view class="u-margin-top-30 u-margin-bottom-15">
				    <u-tag class="u-padding-10" :text="item" v-for="(item, key) in keys" 
					:key="key" type="primary" @click="toNav(item)" />
				</view>
            </view>

            <!--推荐搜索-->
            <view class="u-m-t-40" v-show="recommend && recommend.length > 0">
                <u-section title="推荐搜索" :right="false" color="#fff"></u-section>
                <view class="u-margin-top-30 u-margin-bottom-15">
                    <u-tag class="u-padding-10" :text="item" v-for="(item, key) in recommend" 
					:key="key" type="primary" @click="toNav(item)" />
                </view>
            </view>
        </view>
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
                keys: [],
                key: '',
                navType: 'toNav',
                focus: true,
				deleteView:false
            }
        },
        computed: {
            recommend() {
                return this.$store.state.config.recommendKeys
            }
        },
        methods: {
            //搜索
            search() {
                let keys = this.key;
                if (keys != '') {
                    let search_key = this.$db.get('search_key');
                    if (!search_key) {
                        search_key = [];
                    }
                    let flag = true;
                    for (var key in search_key) {
                        if (search_key[key] == keys) {
                            flag = false;
                        }
                    }
                    if (flag) {
                        search_key.unshift(keys);
                    }
                    this.$db.set('search_key', search_key);
                    this.$db.set('search_term', keys);
					uni.switchTab({
						url:'../category/list/list?key=' + keys
					})
                    // this.$u.route('pages/category/list/list?key=' + keys);
                } else {
                    this.$refs.uToast.show({
                        title: '请输入要查询的关键字',
                        type: 'default',
                    })
                }
            },

            //清除
            deleteKey () {
                //删除显示
                this.keys = [];
                //删除存储
                this.$db.del('search_key');
            },

            //跳转操作
            toNav (keys) {
                this.$db.set('search_term', keys);
                let search_key = this.$db.get('search_key');
                if (!search_key) {
                    search_key = [];
                }
                var flag = true;
                for (var key in search_key) {
                    if (search_key[key] == keys) {
                        flag = false;
                    }
                }
                if (flag) {
                    search_key.unshift(keys);
                }
                this.$db.set('search_key', search_key);
				uni.switchTab({
					url:'../category/list/list?key=' + keys
				})
                // this.$u.route('../category/list/list?key=' + keys);
            },
        },
        //加载触发
        onShow(e) {
            this.keys = this.$db.get('search_key');
            this.focus = true;
        },
        //页面卸载触发
        onUnload() {
            this.$db.set('search_term', '');
        }
    }
</script>

<style lang="scss">
	.search_wrap{
		padding:0 30rpx;
	}
</style>
