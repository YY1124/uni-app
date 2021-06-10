<template>
	<view class="preac-admin-QR">
		<view class="top">
			<view class="top-title">
				保存二维码即可进行签到 
			</view>
			<view class="sub-title">
				充分享受宣讲仪式感
			</view>
		</view>
		<view class="img">
			<view class="img-box">
				<image  class="img-QR" src="../../static/preach/cc.png" mode=""></image>
			</view>
		</view>
		<view class="button" @click="sign">
			保存二维码图片
		</view>
	</view>
</template>

<script>
	import { request } from "../../common/request";
	export default {
		data() {
			return {
			id:'',
			info:{
				Id:'',
				SiteId:'',
				RecordId:'',
				UserPhone:'',
				UserId:'',	
				UserName:'',
				UserPhoto:'',
				OrgId:'',
				OrgName:'',
				WxUserId:'',
				WxNickName:'',
				WxPhoto:"",
				SignInTime:'',
				SignOutTime:'',
				Status:'',
				CreateTime:'',
				UpdateTime:''
			},
			}
		},
		methods: {
		  sign(){ 
			this.info.UpdateTime=this.getTime()
			if(this.info.Status==0){
				this.info.SignInTime=this.getTime()
				this.info.Status=1
			}else if(this.info.Status==1){
				this.info.SignOutTime=this.getTime()
				this.info.Status=2
			}
			this.save() 
		  },
		  getTime(){
		  	var date = new Date()
		  	var month=date.getMonth()+1
		  	if (month < 10)
		  	  month = "0" + month
		  	var hour=date.getHours()
		  	if (hour < 10)
		  	  hour = "0" + hour
		  	var minutes=date.getMinutes()
		  	if (minutes < 10)
		  	   minutes = "0" + minutes
		  	var seconds=date.getSeconds()
		  	if (seconds < 10)
		  	    seconds = "0" + seconds
		  	var time=date.getFullYear() + "-" + month + "-" + date.getDate() +" " + hour + ":" + minutes + ":" + seconds
		  	return time
		  },
		  save(){
		    request({
		  	url:'http://192.168.100.126:8764/propagandaSign/signup/update',
		  	body:this.info,
		  	method:'POST'
		  	}).then((res)=>{
		  		uni.showToast({
		  			title:'签到成功'
		  		}),
				uni.navigateBack(1)
		  	})
		  },
		  getSignDetail(id){
		  		request({
		  			url:'http://192.168.100.126:8764/propagandaSign/signup/select',
		  			query:{
		  			recordId:id,
		  			userId:'',
		  			signupStatus:'',
		  			pageIndex:'',
		  			pageSize:'',
		  			keywords:''	
		  			}
		  		}).then((res)=>{
					this.info=res.Data[0]
		  		})
		  	}
		  },
	
		mounted() {
		  this.getSignDetail(this.id)	
		},
		onLoad(options) {
		this.id=options.id
		}
	}
</script>

<style lang="less" scoped>
.preac-admin-QR{
	width:100%;
	height: 100%;
	padding: 1upx;
	
	>.top{
		width: 360upx;
		height: 83upx;
		margin: 91upx auto;
		text-align: center;
		
		>.top-title{
			font-size: 32upx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #000000;
			margin-bottom:20upx ;
		}
		>.sub-title{
			font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: rgba(102, 102, 102, 1);
		}
	}
	
	>.img{
	  width:420upx;
	  height:420upx;
	  margin: 75upx auto;
	  border:1px dashed #000000;
	 
	  
	  >.img-box{
		 background: url(../../static/preach/kk.png) no-repeat 8upx 7upx;
		 background-size: 400upx;
		 
		 >.img-QR{
			width:238upx;
			 height:238upx;
			 margin: 79upx 85upx  83upx 77upx;
		 }
	  }
	}
	>.button{
		width: 497upx;
		height: 75upx;
		margin: 96upx auto;
		background-color: rgba(233, 70, 61, 1);
		font-size: 28upx;
		text-align: center;
		line-height: 75upx;
		color:#FFFFFF;
		border-radius: 38upx;
	}
	
}

</style>
