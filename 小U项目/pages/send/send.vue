<template>
	<view class="box">
		<view class="row">
			<text>手机号</text>
			<input type="text" v-model="phone" maxlength="12"/>
		</view>
		<view class="row">
			<view class="send">
				<text>验证码</text>
				<text @click='infomation'>发送</text>
			</view>
			<input type="text" v-model="code"  placeholder="- - - -" maxlength="4" />
		</view>
		<view class="row">
			<text style="font-size: 23rpx;">收不到验证码？试试 <text style="color:#00BB00;font-size: 23rpx;"> 语音验证</text></text>
			<!--   -->
			<button type="primary" style="width: 90%;border-radius: 80rpx;margin-top: 50rpx;" @click='Login'>登录</button>
		</view>
		<view class="row">
			<text style="color: #006699;text-align: center;" >通过微信授权登录</text>
		</view>
	</view>
</template>

<script>
	import {myRequest} from '../../utils/request.js'
	export default{
		
			data(){
				return{
					phone:'',
					code:''
				}
			},
			methods:{
			  async	infomation(){
					var result=await myRequest('/api/sms',{phone:this.phone})
					// console.log(result)
					var code=result[1].data.list.code
					uni.setStorageSync("code",code)
					
				},
				async Login(){
					//检验验证码输入
					let codeS=uni.getStorageSync("code")
					if(this.code==codeS){
						var reult=await myRequest('/api/wxlogin',{phone:this.phone})
						// console.log(reult)
						var userinfo=reult[1].data.list
						uni.setStorageSync("userinfo",userinfo)
						uni.switchTab({
							url:'/pages/index/index'
						})
					}else{
						uni.showToast({
							title:'验证码错误',
							icon:'none'
						})
					}
					
				}
			},
			mounted:{
				
			}
	
	}
</script>

<style>
	/* 导入外部的样式文件 */
	/* @import url("../../common/css/index.css"); */
	.box{
		position: relative;
	}

	.row{
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		margin: 40rpx;
	}
	.send{
		display: flex;
		justify-content: space-between;
	}
	.send text:nth-of-type(2){
		color: #00BB00;
	}
	text{
		font-size: 27rpx;
		margin-bottom: 10rpx;
	}
	input{
		border-bottom: 1rpx solid gray;
		height: 65rpx;
	}
	
</style>
