<template>
	<view class="container">
		<!-- left左侧列表 -->
		<view class="left">
			<!-- 循环遍历的 -->
			<view :class="index==navIndex?'left_list activeList':'left_list'" v-for='(item,index) in navList' :key='item.id' @click='rigthDetail(index)'>
				<label for="">{{item.catename}}</label>
			</view>
			
		</view>
		<!-- right右侧详细分类商品 -->
		<view class="right">
			<!-- 每一个小类 -->
			<view class="right_list" >
				<!-- 商品 -->
				<view class="bottom" >
					<view class="bottom_list" v-for="item in navList[navIndex].children" :key='item.id' @click="toProduct(item.id)">
						<!-- 每个最多显示6个 -->
						<view >
							<image :src="item.img" alt="">
						</view>
						<view class="title">
							<span>{{item.catename}}</span>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		
		
		
	</view>
</template>

<script>
	import {myRequest,baseUrl} from '../../utils/request.js'
	export default {
		data(){
			return {
				navIndex:0,
				navList:[] //左侧导航栏
			}
		},
		methods:{
			rigthDetail(index){
				// console.log(index)
				this.navIndex=index;
				
			},
			//------去商品列表页--------
			toProduct(id){
				uni.navigateTo({
					url:'/pages/product/product?fid='+id
				})				
			}
		},
		async onLoad(){
			let result=await myRequest('/api/getcates')
			console.log(result)
			
			result[1].data.list.forEach(item=>{
				if(item.children!=undefined)
				   item.children.forEach(item=>{
					    item.img=baseUrl+item.img
				    })
			})
			this.navList=result[1].data.list
		}
	}
</script>

<style>
	/* 导入外部的样式文件 */
	@import url("../../common/css/classify.css");
	
	/* 点击左侧导航，显示动态样式 */
	.activeList{
		border-left: 6rpx solid #f26b11;
		color: #f26b11;
	}
</style>
