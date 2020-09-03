<template>
	<view class="container">
		<!-- 详情图片 -->
		<view class="detailImage">
			<image :src="detailList.img">
		</view>
		<!-- 详情基本信息 -->
		<view class="detailInfo">
			<view class="detailInfoName">
				<label>{{detailList.goodsname}}</label>
			</view>
			<view class="detailInfoPrice">
				<label style="color: red;">￥ {{detailList.price}}</label>
				<label style="font-size: 24rpx;color: #ccc;">(此价格不与套装优惠同时享受)</label>
			</view>			
		</view>
		
		<!-- 基本信息改变 -->
		<view class="changeInfo">
			<view class="youhui">
				<view>促销：<label class="manjian">满减</label><label class="dazhe">满2件9折；3件8折</label></view>
			</view>
			<!-- 数量加减 -->
			<view class="changeNum">
				<view class="num">购买数量</view>
				<view class="action">
				  <label class="jian" @click='jianS'>-</label>
				  <label class="zhi">{{num}}</label>
				  <label  class="jia" @click='add'>+</label>
				</view>
			</view>
			<!-- 商品属性 -->
			<view class="changeState">
				<view class="productStat">
					<label for="">商品属性</label>
				</view>
				<view class="productGuige">
					<label>{{detailList.specsname}}</label>
					<view class="threed3i1" v-for='item in detailList.specsattr' :key='item.id'>{{item}}</view>
					<!-- <view class="threed3i2">5g</view> -->
				</view>
			</view>
		</view>
		<!-- 商品详情 -->
		 <view class="productDetail">
			<view class="productDetailTitle">商品详情</view>
			<!-- 需要处理图片自适应问题 -->
			<!-- <image src="/static/detail/2.jpg" mode="aspectFill"/> -->
			<rich-text :nodes='detailList.description'></rich-text>
		</view>
		
		<view class="productEval">
			<view class="eval">商品评价</view>
			<view>
				<label class="evalUsername">小李白</label>
				<label class="evalUsertitle">效果很好，物流到位，下次继续来</label>					
				<view class="evalImage">
					<image src="../../static/detail/pingjia1.jpg"/>
					<image src="../../static/detail/pingjia2.jpg"/>
					<image src="../../static/detail/pingjia3.jpg"/>
						
				</view>
				<label class="evalTime">2020-01-13</label>
			</view>
		</view>
		<!-- 底部按钮 -->
		<view class="foot">
			<button class="footbtn1" @click='shop_car'>加入购物车</button>
			<button class="footbtn2">立即购买</button>
		</view>
	</view>
</template>
<script>
	import {myRequest,baseUrl} from '../../utils/request.js'
	export default{
		data(){
			return{
				detailList:[],  //详情
				num:1, //数量
				id:0
			}
		},
		methods:{
			jianS(){
				this.num-=1
				if(this.num<=1){
					this.num=1;
				}
				
			},
			add(){
				this.num+=1
			},
			async shop_car(){
				let{uid,token}=uni.getStorageSync('userinfo')
				// console.log(uid,token)
				let result1=await myRequest('/api/checktoken',{},'GET',{authorization:token})
				if(result1[1].data.code==403){
					uni.showToast({
						title:'请先登录',
						icon:'none'
					})
				}else{
					let result=await myRequest('/api/cartadd',{uid:uid,goodsid:this.id,num:this.num},'GET',{authorization:token})
					uni.showToast({
						title:'添加成功'
					})
				}
				
			}
		},
		async onLoad(e){
			// console.log(e)
			var id=e.id
			this.id=id
			var result=await myRequest("/api/getgoodsinfo",{id:id})
			// console.log(result)
			if(result[1].data.code==200){
				this.detailList=result[1].data.list[0]
				this.detailList.img=baseUrl+this.detailList.img
				this.detailList.specsattr=this.detailList.specsattr.split(',')
				
				
			}
		}
	}
</script>
<style>
	@import url("../../common/css/details.css");
	
</style>
	
