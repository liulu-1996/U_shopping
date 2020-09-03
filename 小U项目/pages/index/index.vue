<template>
	<view class="">
		<!-- 顶部信息 -->
		<view class="HomeTop">
		  <view class="logoView">
			<!-- logo -->
			<img class="logo" src="/static/index/logo.jpg" alt="" />
		  </view>
		  <view class="inputView" style='display: flex;'>
			<input class="textinput" style="center" type="text" v-model='inpu'  placeholder="寻找商品"/>
			<text @click='searc' style='background-color: #808080'>搜索</text>
		  </view>
		</view>
		
		<!-- 顶部导航  scroll-view -->
		<view class="tabs">
		    <scroll-view id="tab-bar" class="scroll-h" :scroll-x="true" :show-scrollbar="false" >
		        <view class="uni-tab-item"  data-current="0"  @click='navChange(0)'>
		           <text :class="navIndex==0?'uni-tab-item-title uni-tab-item-title-active':'uni-tab-item-title'" >
						推荐
					</text>
		        </view>
				<view class="uni-tab-item"  v-for='(item,index) in Nav' :key='item.id' @click='navChange(index+1)'>
					<text  :class="index+1==navIndex?'uni-tab-item-title-active':'uni-tab-item-title'">
						{{item.catename}}
					</text>
				</view>
		    </scroll-view>
			<!--  -->
			       
					
			   
			
		</view>
		<!-- 推荐轮播图 -->
		<view class="">
			<swiper :indicator-dots="true" :autoplay="true" :interval="2000" :duration="1000">
				<swiper-item v-for='(item,index) in list' :key='item.id'>
					<view class="swiper-item">
						<image  :src="item.img" mode=""></image>
					</view>
				</swiper-item>
				
				
			</swiper>
		</view>
		
		<!-- 功能导航开始 -->
		<!-- 功能导航 -->
		<view class="FunctNavCon">
		  <view class="FunctNavLi">
			<image src="../../static/index/xiaohuoban.png" alt class="FunImg" />
			<p>限时抢购</p>
		  </view>
		  <view class="FunctNavLi">
		  			<image src="../../static/index/jifentixicopy.png" alt class="FunImg" />
		  			<p>积分商城</p>
		  </view>
		  <view class="FunctNavLi">
		  			<image src="../../static/index/lianxiwomen.png" alt class="FunImg" />
		  			<p>联系我们</p>
		  </view>
		  <view class="FunctNavLi">
		  			<image src="../../static/index/-shangpinfenlei-gai.png" alt class="FunImg" />
		  			<p @click='sort'>商品分类</p>
		  </view>
		</view>
		
		<!-- 推荐部分 未写 -->
		<view class="HotGoods">
			<!-- 热推左侧 -->
			<view class="HotLeft">
				<view class="LimitedContent">
				  <image class="LimitedImg" src="../../static/index/xianshi.jpg" alt />
				  <label class="Limited">限时秒杀</label>
				</view>
				<p class="LimitedTitle">每天零点场，好货秒不停</p>
				<!-- 倒计时 -->
				<view class="LimitTimeAll">
				  <label class="LimitTime">{{hour}}</label>
				  <view class="maohao">:</view>
				  <label class="LimitTime">{{minute}}</label>
				  <view class="maohao">:</view>
				  <label class="LimitTime">{{second}}</label>
				  <label class="Seckill">秒杀</label>
				</view>
				<image src="../../static/index/goods2.jpg" class="LimitedgoodsImg" alt="">
			</view>
			
			<!-- 热推右侧 -->
			<view class="HotRight">
				<!-- 右侧顶部部分 -->
				<view class="HotTop">
					<view class="HomeTopInfo">
						<view class="HomeTopInfoOne">
							<label class="BrandNew">品牌上新</label>
							<label class="Discount">折</label>
						</view>
						<view class="HomeTopRobbig">
							<label>每日九点，抢大牌</label>
						</view>
					</view>		
					<!-- <div> -->
					<view class="Cloth">
						<image  src="../../static/index/goods1.jpg" alt="">
					</view>
					
					<!-- </div> -->				
				</view>
				<!-- 右侧底部 -->
				<view class="HotBottom">
					<view class="HotBottomLeft">
						<view class="HotBottomLeftInfo">
							<label class="HotBottomLeftInfoTitle">每日十件</label>
							<label class="HotBottomLeftInfoDetail">只为你选好货</label>
						</view>
						<view class="HotBottomLeftImage">
							<image src="../../static/index/goods1.jpg" mode=""></image>
						</view>
					</view>
					<view class="HotBottomRight">
						<view class="HotBottomRightInfo">
							<label class="HotBottomRightInfoTitle">每日十件</label>
							<label class="HotBottomRightInfoDetail">只为你选好货</label>
						</view>
						<view class="HotBottomRightImage">
							<image src="../../static/index/goods1.jpg" mode=""></image>
						</view>
					</view>
				</view>
			</view>
		</view>
		  <!-- 轮播图2 -->
		<div class="Swiper2">
		  <image class="Swiper2Img" src="../../static/index/swiper1.jpg" alt />
		  
		</div>
		
		<!-- 底部商品列表 -->
		<view class="products">
			<!-- 商品标签 -->
			<view class="tags">
				<view :class="tabIndex==0?'active_tag_list':'tag_list'" @click='tabchange(0)'>
					<label for="" >热门推荐</label>
				</view>
				<view :class="tabIndex==1?'active_tag_list':'tag_list'" @click='tabchange(1)'>
					<label for="">最新商品</label>
				</view>
				<view :class="tabIndex==2?'active_tag_list':'tag_list'" @click='tabchange(2)'>
					<label for="">只看专场</label>
				</view>
				
			</view>
			<!-- 标签对应的商品 -->
			<view class="tags_product">
				<view class="product" v-for='(item,index) in goodList[tabIndex].content' :key='index' @click='detail(item.id)'>
					<view class="GoodsLeft">
						<image class="GoodsImg" :src="item.img" alt />
					</view>
					<view class="GoodsCont">
						<view class="GoodConTit">{{item.goodsname}}</view>
						<view class="GoodsPrice">￥{{item.price}}</view>
						<view class="yimai">已售8000件</view>
						<view class="Immed">立即抢购</view>
					</view>
				</view>
				
				
			</view>
		</view>
	</view>
</template>
 
<script>
	import {myRequest,baseUrl} from '../../utils/request.js'
	export default{
		
			data(){
				 return{
					 list:[], //轮播图
					 Nav:[],  //导航栏
					 navIndex:0,
					 hour:0,
					 minute:0,
					 second:0,
					 tabIndex:0 ,//选项卡
					 goodList:[], //商品列表
					 inpu:'' //输入框的值
				 }
				 
			},
			methods:{
				//--------导航选项卡-------------
				navChange(index){
						// console.log(index)
						this.navIndex=index
						uni.navigateTo({
							url:'../product/product'
						})
				},
				//--------选项卡-------------
				tabchange(index){
							  this.tabIndex=index
				},
				//---------------详情---------------
				detail(id){
					// console.log(id)
					uni.navigateTo({
						url:'../details/details?id='+id
					})
				},
				//------------商品分类-----------------
				 sort(){
					 uni.navigateTo({
					 	url:'../classify/classify'
					 })
				 },
				 //-------------搜索--------------
				 searc(){
					 uni.navigateTo({
					 	url:'/pages/search/search?keywords='+this.inpu
					 })
				 }
			},
			async onLoad(){
				
			//------------轮播图--------------------
			  var result=await myRequest("/api/getbanner")
				
				if(result[1].data.code==200){
					this.list=result[1].data.list
					this.list.forEach(item=>{
						item.img=baseUrl+item.img
					})
				}
			//---------------导航栏---------------------- 	
			 var resultNav=await myRequest('/api/getcate')
		       // console.log(resultNav)
			   if(resultNav[1].data.code==200){
				   this.Nav=resultNav[1].data.list
			   }
			   // console.log(this.Nav.current)
			   
			//---------------秒杀--------------------------	
			var resultMx=await myRequest("/api/getseckill")
			setInterval(()=>{
				var endtime=resultMx[1].data.list[0].endtime
				var nowtime=new Date().getTime()
				var distance=endtime-nowtime
				distance=Math.floor(distance/1000)
				var hour=Math.floor(distance/3600)
				var minute=Math.floor(distance%3600/60)
				var second=distance%3600%60
				this.hour=hour<10?"0"+hour:hour,
				this.minute=minute<10?'0'+minute:minute,
				this.second=second<10?"0"+second:second
			},1000)
			
			// console.log(endtime)
			//------------------商品列表---------------------------
			   
			    var resultGood=await myRequest('/api/getindexgoods')
				// console.log(resultGood)
			      resultGood[1].data.list.forEach(item=>{
					  item.content.forEach(item=>{
						  item.img=baseUrl+item.img
					  })
				  })
				  this.goodList=resultGood[1].data.list
				  // console.log(this.goodList)
			},
			
		
	}
	
</script>

<style>
	/* 导入外部的样式文件 */
	@import url("../../common/css/index.css");
</style>
