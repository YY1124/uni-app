<template>
	<view class="preach-user-detail-on">
		<substance :Id="list" :time="time"></substance>
		<declare :Id="list"></declare>
		<view class="img-box">
			<view class="img1" v-for="(item,index) in imgUrl" :key="index">
				<image class="image" :src="item.url"></image>
			</view>
		</view>
		<view class="bottom">
			<view class="bottom-left">
				<view class="time-title">
				距离宣讲开始时间
				</view>
				<view class="time">
					<text class="text">
			        {{timer.day || '00'}}天
			         </text> 
					<text class="text">
			        {{timer.hour || '00'}}时
			        </text> 
					<text class="text">
					{{timer.min || '00'}}分
					</text> 
					<text class="text">
					{{timer.sec || '00'}}秒
					</text>
				</view>
			</view>
			<view class="bottom-right" @click="enroll">
				<view class="button-box">
					{{cue}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import { USER_DETAILS,getUserInfo, } from "../../../common/common";
	import { request } from "../../../common/request";
	import substance from"../components/substance.vue"
	import declare from"../components/declare.vue"
	import signed from "../components/signed.vue"
	export default {
		data() {
			return {
			list:'',
		    id:'',
			cue:"我要报名",
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
			time:'',
			timer: {},
			startTime:'',
			endTime:"",
			isStart: false,
			imgUrl:'',
			Status:'',
			}
		},
		components:{
			substance,
			declare
		},
		methods: {
			timeFormat(param) {
							return param < 10 ? '0' + param : param;
						},
			countDown () {
				var interval = setInterval(() => {
								// 当前时间
								let nowTime =new Date(this.getTime().replace(/-/g,'/')).getTime()
								//console.log(nowTime)
								let endTime =new Date(this.list.EndTime.replace(/-/g,'/')).getTime()
								//console.log(endTime)
					            let startTime = new Date((this.list.StartTime.replace(/-/g,'/'))).getTime();
								//console.log(nowTime-startTime)
								// 已开始
								if (startTime - nowTime <= 0) {
									if (endTime - nowTime < 0) { 
										clearInterval(interval)
										}
			                        if (endTime - nowTime <= 0) {
									   // 计时结束
									    clearInterval(interval)
								    } 
								} else if (startTime - nowTime > 0) {  // 即将开始
									this.isStart = false
									let timer = (startTime - nowTime) / 1000;
									let day = parseInt(timer / (60*60*24));
									let hour = parseInt(timer % (60*60*24) / 3600);
									let min = parseInt(timer % (60*60*24) % 3600 / 60);
									let sec = parseInt(timer % (60*60*24) % 3600 %60);
									this.timer = {
										day: this.timeFormat(day),
										hour: this.timeFormat(hour),
										min: this.timeFormat(min),
										sec: this.timeFormat(sec)
									}
								}else if(startTime - nowTime >= 0){
										
								}
							}, 1000)
						},
			save(){
			    request({
				url:'http://192.168.100.126:8764/propagandaSign/signup/save',
				body:this.info,
				method:'POST'
				}).then((res)=>{
					uni.showToast({
						title:'报名成功'
					})
				})
			},
			Week(y,m,d){
					 var myDate=new Date();
					myDate.setFullYear(y,m-1,d);
					var week = myDate.getDay()
					switch(week){
					case 0:
					return '星期日';
					case 1:
					return '星期一';
					case 2:
					return '星期二';
					case 3:
					return '星期三';
					case 4:
					return '星期四';
					case 5:
					return '星期五';
					case 6:
					return '星期六'; 
					}
				},
			getDetail(id){
					request({
						url:"http://192.168.100.126:8764/propaganda/detail",
						query:{
							recordId:id
							  }
								}).then((res)=>{
								 console.log(res.Data)
								 this.list=res.Data
								 this.time={
								 		"year":this.list.StartTime.substring(0,4),
								 		"month":this.list.StartTime.substring(5,7),
								 		"day":this.list.StartTime.substring(8,10),
								 		"Starthour":this.list.StartTime.substring(11,16),
								 		"Endhour":this.list.EndTime.substring(11,16),
										"week":this.Week(this.list.StartTime.substring(0,4),this.list.StartTime.substring(5,7),this.list.StartTime.substring(8,10),)
								 	}
						if((res.Data.RecordImagesUrl)==null){
							this.imgUrl=[]
						}else{
							this.imgUrl=JSON.parse(res.Data.RecordImagesUrl)
						}
									
					 })
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
			enroll(){
			if(this.info.Status==null){
			   this.info.CreateTime=this.getTime()
			   this.info.Status=0
			   this.cue="报名成功"
			   console.log(this.info)
			   this.save()
			}else{
				uni.showToast({
					title:'你已经报名'
				})
			}
		   
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
					if(res.Data[0]==undefined){
						this.info.Status=null
					}else{
						if(res.Data[0].Status!==null){
							this.info.Status=res.Data[0].Status
							this.cue="报名成功"
						}else{
							this.info.Status=null
						}
					}
					
				})
			}
		},
		mounted() {
			this.getDetail(this.id)
			this.getSignDetail(this.id)
			this.countDown()
		},
		onShow() {
			this.list=""
			this.getDetail(this.id)
			this.getSignDetail(this.id)
		},
		onLoad(options) {
			this.id=options.Id
			this.number=options.number
			this.info.RecordId=options.Id
			this.info.OrgId=getUserInfo().OrgId
			this.info.OrgName=getUserInfo().OrgName
			this.info.UserName=getUserInfo().UserTrueName
			this.info.SiteId=getUserInfo().SiteId
			this.info.UserId=getUserInfo().UserId
			this.info.UserPhone=getUserInfo().UserMobilePhone
		}
	}
</script>

<style lang="less" scoped>
.preach-user-detail-on{
	width:100%;
	height: 100%;
	padding: 1upx;
	
	>.img-box{
		width:90%;
		height: 218upx;
		
		margin: -40upx 0 180upx 32upx;
		
		>.img1{
			width:49%;
			height: 218upx;
			float:left;
			
			>.image{
				width:100%;
				height: 218upx;
			}
		}
		
		
	}
	>.bottom{
		width:100%;
		height: 98upx;
		padding: 1upx;
		position: fixed;
		bottom: 0;
		left: 0;
		background-color: #FFFFFF;
		z-index: 20;
		
		
		>.bottom-left{
			width:50%;
			height: 100%;
			float: left;
			
			>.time-title{
				margin:20upx 0 16upx 32upx;
				font-size: 22upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #333333;
			}
			>.time{
				margin:5upx 0 16upx 32upx;
			  >.text{
				  font-size: 28upx;
				  font-family: PingFang SC;
				  font-weight: bold;
				  color: #333333;
				  margin-right: 10upx;
			  }
			}
		}
		
		>.bottom-right{
			width:50%;
			height: 100%;
			float: right;
			position: relative;
			
			>.button-box{
				width:200upx;
				height: 72upx;
				position: absolute;
				right:24upx;
				bottom:13upx;		
	            background: #E9463D;
				line-height: 72upx;
				text-align: center;
				color:#FFFFFF;
				border-radius: 6upx 36upx 6upx 36upx;
				
				
			}
		}
	}
}
</style>
