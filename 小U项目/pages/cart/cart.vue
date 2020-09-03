<template>
	<view class="container">
		<view class="top">
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="0" v-for='(item,index) in carList' :key='item.id'>
				<view class="cartInfo" id="demo1" @click='detailTo(index)'>
					<!-- 购物车商品信息 -->
					<view class="cartInfochild">
						<!-- 选择框 -->
						<view class="cartInfo_switch common">
							<!-- 点击当前商品 -->
							<switch type="checkbox" :checked="true"  @change="singeChecked($event,index)"/>
						</view>
						<!-- 图片 -->
						<view class="cartInfo_image common"><image :src="item.img" mode=""></image></view>

						<!-- 信息 -->
						<view class="cartInfo_info">
							<label for="" style="font-size: 26rpx;">{{item.goodsname}}</label>
							<label for="" style="font-size: 24rpx; color: #ccc;">规格：黑色</label>
							<label for="" style="color: red;">￥{{item.price}}</label>
						</view>
						<!-- 加减数量 -->
						<view class="cartInfo_num">
							<view class="cartInfo_num_child">
								<label for="" @click='jianS(index)'>-</label>
								<label for="">{{item.num}}</label>
								<label for="" @click='add(index)'>+</label>
							</view>
						</view>
					</view>
				</view>

				<view class="cartDel" id="demo2" @click='del(index,item.id)'><label for="">删除</label></view>
			</scroll-view>
		</view>

		<!-- 底部 -->
		<view class="foot">
			<view class="footd1">
				<switch type="checkbox" :checked="isChecked" />
				<label>全选</label>
			</view>
			<view class="footd2">
				<view class="footd2sp1">
					总计：
					<label style="color: red;" >{{total}}</label>
				</view>
				<label class="footd2sp2">不含运费，已优惠￥0.00</label>
			</view>
			<!-- 跳到提交订单，结算页面 -->
			<view class="footd3">
				<!-- 被选中的商品的个数 -->
				<label @click='jiesD'>去结算</label>
			</view>
		</view>
	</view>
</template>

<script>
	import {myRequest,baseUrl} from '../../utils/request.js'
export default {
	data(){
		return{
			carList:[],//购物车列表
			 jieJ:0
		}
	},
	methods:{
		//---数量减少------
		jianS(index){
			//------修改页面数量--------
			this.carList[index].num-=1
			if(this.carList[index].num<=1){
				this.carList[index].num=1
			}
		    //-------调取修改数据库数量---------
			 this.editNum(index)
		},
		//---数量增加------
		add(index){
			this.carList[index].num+=1
			//-------调取修改数据库数量---------
			 this.editNum(index)
		},
		 //-------修改数据库数量---------
		 async editNum(index){
			 let {id,num,checked}=this.carList[index]
			 checked=checked?1:0 //与数据库保持一致
			 let {token}=uni.getStorageSync('userinfo')
			 let reult=await myRequest('/api/cartedit',{id:id,num:num,checked:checked},"GET",{authorization:token})
		 },
		 //--------------修改单选框---------------
		 singeChecked(e,index){
			 // console.log(e)
			 this.carList[index].checked=e.detail.value
			 this.editNum(index)
			 
			 
		 },
		 //--------跳转到结算页--------------
		 jiesD(){
			 var jiesList=this.carList.filter(item=>{
				 return item.checked==true
			 })
			 // console.log(jiesList)
			 uni.setStorageSync('jiesList',jiesList)
			 uni.navigateTo({
			 	url:'../confirm/confirm'
			 })
		 },
		 //-----------删除数据----------------
		 async del(index,id){
			 // console.log(index,id)
			 let {token} =uni.getStorageSync('userinfo')
			  let result=await myRequest('/api/cartdelete',{id:id},"GET",{authorization:token})
			 this.carList.splice(index,1)
			 this.carList=this.carList
			 
		 },
		 //--------跳转到详情页面--------------
		 detailTo(index){
			 let id=this.carList[index].goodsid
			 // console.log(id)
			 uni.navigateTo({
			 	url:'/pages/details/details?id='+id
			 })
		 }
		 
	},
	async onShow(){
		 //----------判断token是否过期----------
		 let {uid,token}=uni.getStorageSync("userinfo")
		 // console.log(token)
		 let result1=await myRequest('/api/checktoken',{},'GET',{authorization:token})
		 // console.log(result)
		 if(result1[1].data.code==403){
			 uni.showToast({
			 	title:'请先登录',
				icon:'none'
			 })
		 	uni.navigateTo({
		 		url:'../send/send'
		 	})
		 }else{
			 //-----------购物车列表----------------
			 // let {uid,token}=uni.getStorageSync("userinfo")
			 let result=await myRequest('/api/cartlist',{uid:uid},"GET",{authorization:token})
			 // console.log(result)
			 let list=result[1].data.list
			 
			 if(list!=null){
			 	list.forEach(item=>{
			 		item.checked=item.checked==1?true:false
			 		item.img=baseUrl+item.img
			 	})
			 	this.carList=list
			 }
			 // console.log(this.carList)
		 }
		
		
	},
	computed:{
		//--------计算总价-------------
		total(){
			let sum=0;
			this.carList.forEach(item=>{
				if(item.checked){
					sum+=item.price*item.num
				}
			})
			return sum
		},
		//-------是否全选----------
		isChecked:{
			get:function(){
				return this.carList.every(item=>{
					return item.checked==true
				})
			},
			set:function(val){
				this.carList.forEach(item=>{
					item.checked=val
				})
			}
			
		}
	}
};
</script>

<style>
@import url('../../common/css/cart.css');
.cartInfo_image_img {
	width: 120rpx;
	height: 120rpx;
}
</style>
