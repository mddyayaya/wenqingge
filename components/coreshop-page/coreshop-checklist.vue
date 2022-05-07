<template>
	<view class="check-list" v-if="checkList.length > 0">
		<u-tabs 
			:current="current"
			active-color="#fff"
			inactive-color="#999999"
			:list="checkList" 
			height="60rpx"
			font-size="24rpx"
			bgColor="rgb(38,46,57)"
			@change="selectType"
			@click="typeChange">
			<view
				slot="right"
				style="padding-left: 4px;"
				@tap="$u.toast('插槽被点击')">
				更多
			</view>
		</u-tabs>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				current:0,
				checkList: [{
                    name: '推荐活动',
                }]
			}
		},
		props:{
			typeList:{
				type:Array,
				default:[]
			}
		},
		watch:{
			typeList(value){
				let arr = []
				value.forEach(item =>{
					arr.push({name:item})
				})
				this.checkList = this.checkList.concat(arr);
				this.$emit("recommendAvtivityList")
			}
		},
		methods:{
			selectType(e){
				this.current = e
				if(this.current == 0){
					// 推荐活动
					this.$emit("recommendAvtivityList")
				}else{
					this.$emit("initAvtivityList",this.checkList[e].name)
				}
			},
			typeChange(e){
				this.current = e
				console.log(e)
				if(this.current == 0){
					// 推荐活动
					this.$emit("recommendAvtivityList")
				}else{
					this.$emit("initAvtivityList",this.checkList[e].name)
				}
			}
		}
	}
</script>

<style>
	.check-list{
		width: 100%;
		height: 60rpx;
		line-height: 60rpx;
		background-color: #fff;
	}
</style>
