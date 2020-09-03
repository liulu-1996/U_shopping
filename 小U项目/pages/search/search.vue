<template>
	<view>
		<uni-search-bar :radius="100" bgColor="#ffffff" class="input_"></uni-search-bar>
		<view class="list" v-if='flag'>
			<view class="row" v-for='item in list' :key='item.id' @click='toDetail(item.id)'>
				<image :src="item.img" mode="widthFix"></image>
				<view class="info">
					<text>{{item.goodsname}}</text>	
					<text>原价￥{{item.market_price}}</text>	
					<text>现价￥{{item.price}}</text>	
					<text>3565评论</text>	
				</view>
			</view>
			
		</view>
		<view class="tishi" v-else>
			没有数据亲！
		</view>
	</view>
</template>

<script>
	import {myRequest,baseUrl} from '../../utils/request.js'
	export default{
		data(){
			return {
				list:[],
				flag:false
			}
		},
		methods:{
			//----------跳转详情页面---------------
			toDetail(id){
				uni.navigateTo({
					url:'/pages/details/details?id='+id
				})
			}
		},
		async onLoad(e){
			// console.log(e)
			let keywords=e.keywords
			let result=await myRequest('/api/search',{keywords:keywords})
			// console.log(result)
			if(result[1].data.code==200){
				let list=result[1].data.list
				list.forEach(item=>{
					item.img=baseUrl+item.img
				})
				this.list=list
				// console.log(this.list)
			}
			if(this.list.length>0){
				this.flag=true
			}
		}
	}
</script>

<style>

	.row{
		position: relative;
		margin: 10rpx 20rpx;
		height: 190rpx;
		border-bottom: 1rpx solid #AAAAAA;
	}
	image{
		position: absolute;
		top: 0;
		left:10rpx;
		width: 160rpx;
	}
	.info{
		height: 85%;
		width: 100%;
		position: absolute;
		top: 0;
		left: 200rpx;
		display: flex;
		justify-content: space-around;
		flex-direction: column;
		font-size: 25rpx;
	}
	.info text:nth-of-type(2){
		color: red;
	}
	.info text:nth-of-type(3){
		font-size: 20rpx;
		color: #AAAAAA;
	}
	.tishi{
		line-height: 200rpx;
		text-align: center;
		font-size: 40rpx;
		color: #006699;
	}
</style>
