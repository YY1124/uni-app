<template>
	<view class="preach-user-sign">
		<view class="content-box" v-for="item in list" :key="item.Id">
			<view class="img-box">
				<image class="img" :src="item.UserPhoto" mode=""></image>
			</view>
			<view class="inform-box">
				<view class="name">
					<text class="text">{{item.UserName}}</text><text class="text1">{{item.UserPhone}}</text>
				</view>
				<view class="time">
					<text class="text2">签到时间：</text><text class="text3">{{item.SignInTime}}</text>
				</view>
				<view class="time">
					<text class="text2">签退时间：</text><text class="text3">{{item.SignOutTime}}</text>
				</view>
			</view>
		</view>
	</view>
</template>
<script>
	import { USER_DETAILS,getUserInfo, } from "../../common/common";
	import { request } from "../../common/request";
	export default {
		data() {
			return {
				url:'http://192.168.100.126:8764/propagandaSign/signup/select',
				list:''
			}
		},
		methods: {
			getSigned(){
				request({
					url:this.url,
					query:{
						recordId:"",
						userId:"",
						signupStatus:"",
						pageIndex:"",
						pageSize:"",
						keywords:""
					}
				}).then((res)=>{
					this.list=res.Data
					console.log(this.list)
				})
			}
		},
		mounted() {
			this.getSigned()
		}
	}
</script>

<style lang="less" scoped>
.preach-user-sign{
	width:100%;
	height: 100%;
	padding:1upx;
	
	>.content-box{
		width:100%;
		height:174upx;
		
		>.img-box{
			width: 20%;
			height:78upx;
			float: left;
			
			
			>.img{
				width: 78upx;
				height:78upx;
				border-radius: 50%;
				margin:51upx 0 53upx 35upx;
			}
		}
		>.inform-box{
		  padding: 1upx;
		  width: 80%;
		  float: right;
		  
		  >.name{
			  margin: 20upx 10upx;
			  
			  
			  >.text{
				 color:#333333; 
				 margin-right: 25upx;
				 font-size: 30upx;
				 font-family: PingFang SC;
				 font-weight: bold;
			  }
			  >.text1{
				 color:#333333;
				 font-size: 30upx;
				 font-family: PingFang SC;
				 font-weight: bold;
			  }
		  }
		  >.time{
			  margin:15upx 10upx;
		  	
		  	>.text2{
		  		color:#666666;
		  		font-size: 30upx;
				margin-right: 25upx;
		  		font-family: PingFang SC;
		  		font-weight: 500;
		  	}
		  	>.text3{
		  		color:#666666;
		  		font-size: 30upx;
		  		font-family: PingFang SC;
		  		font-weight: 500;
		  	}
		  }
		}
	}
	
}
</style>
