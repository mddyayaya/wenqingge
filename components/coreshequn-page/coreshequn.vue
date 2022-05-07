<template>
	<view class="shequnwrap">
		<coreshequnMygroup></coreshequnMygroup>
		<coreshequnMySec></coreshequnMySec>
	</view>
</template>

<script>
	import coreshequnMygroup from './coreshequn-mygroup.vue'
	import coreshequnMySec from './coreshequn-sec.vue'
	export default {
		components:{
			coreshequnMygroup,
			coreshequnMySec
		},
		data(){
			return{
				
			}
		}
	}
</script>

<style>
	.shequnwrap{
		padding:29rpx 30rpx 0 30rpx;
	}
</style>
