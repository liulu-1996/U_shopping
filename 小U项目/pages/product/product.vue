<template>
	<view>
		<view class="search" >
			<icon type="search" color="white" size="10"></icon>
			<input type="text" value="" placeholder="搜索商品" v-model='inp' placeholder-class="placeholder"  />
			<text @click='searchS'>搜索</text>
		</view>
		<view class="list" v-if='flag'>
			<view class="row" v-for='item in list' :key='item.id' @click='toDetail(item.id)'>
				<image :src="item.img" mode="widthFix"></image>
				<view class="info">
					<text>{{item.goodsname}}</text>	
					<text>￥{{item.price}}</text>	
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
			return{
				list:[], //列表
				inp:'',   //输入框的值
				flag:false   //标志
			}
		},
		methods:{
			//-------模糊查询---------------
			async searchS(){
				// console.log(this.inp)
				let reuslt=await myRequest('/api/search',{keywords:this.inp})
				console.log(reuslt)
				
				if(reuslt[1].data.code==200){
					let list=reuslt[1].data.list
					list.forEach(item=>{
						 item.img=baseUrl+item.img
					})
					this.list=list
				}
				if(this.list.length>0){
					this.flag=true
				}
				
			},
			//-----跳到详情页面---------
			toDetail(id){
				uni.navigateTo({
					url:'/pages/details/details?id='+id
				})
			}
		},
		async onLoad(e){
			// console.log(e)
			let fid=e.fid
			let result=await myRequest('/api/getcategoods',{fid:fid})
			// console.log(result)
		}
		
	}
</script>

<style>
	.search{
		margin: 50rpx 20rpx;
		height: 80rpx;
		background-color: #f26b11;
		border-radius: 80rpx;
		color: white;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	icon{
		margin:0 10rpx 0 250rpx;
	}
	.placeholder{
		color: white;
		font-size: 25rpx;
	}
	.row{
		position: relative;
		margin: 10rpx 20rpx;
		height: 330rpx;
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
