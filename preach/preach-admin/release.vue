<template>
	<view class="preach-admin-release">
		<view class="top-box">
			<view class="top-select">
				<text class="text text1">关联宣讲计划</text> <text class="wenhao">?</text>
				<input class="text text2" @click="goTo" type="text" :value="relevance" placeholder="请选择"/>
			</view>
			<view class="top-author">
				<text class="text text1">宣讲人</text>
				<view class="text text3"><input type="text" class="text4" @input="NameInput" :value="info.UserName"></view>
			</view>
		</view>
		<view class="theme">
			<view class="theme-title">
				宣讲主题
			</view>
			<view class="theme-content">
				<input class="content" type="text" @input="onInput" :value="info.Title" placeholder="请输入宣讲主题"/>
			</view>
		</view>
		<view class="address">
			<view class="address-title">
				宣讲地点
			</view>
			<view class="address-content">
				<input class="content" type="text" @input="AddressInput" :value="info.Address" placeholder="请输入宣讲地址" />
			</view>
			<image @click="choose" class="address-img" src="../../../static/preach/dingwei.png"></image>
		</view>
		<view class="time">
			<view class="time-left" @click="showStartTimePicker">
				<text class="text">开始时间</text>
				<input class="text" type="text" @input="StartInput" :value="info.StartTime" placeholder="请输入">
			</view>
			<view class="time-right" @click="showEndTimePicker">
			   <text class="text">结束时间</text>
			   <input class="text" type="text" @input="EndInput" :value="info.EndTime" placeholder="请输入">
			</view>
		</view>
		<view class="preach-content">
			<text class="preach-content-text">宣讲内容</text>
			<image class='preach-content-img' src="../../../static/preach/yuyin.png"></image>
			<view class="iconfont icondingwei icon"></view>
		</view>
		<view class="update">
			<textarea class="update-cue" @input="ContentInput" maxlength="500"  :value="info.Content" placeholder="请输入内容或拍照上传纸质记录" />
			<view class="count">
				{{info.Content.length}}/500
			</view>
			<view class="add" @click="add">
			</view>
			<view class="img-box">
				<view class="img1" @click="del(index)" v-for="(item,index) in RecordImagesUrl" :key="index">
					<image class="img" :src="item.name"></image>
				</view>
			</view>
			<view class="position">
				
			</view>
		</view>
		<view class="bottom" @click="release">
			<view class="bottom-button">
			 发布
			</view>
		</view>
		<w-picker
		  ref="startTimePicker"
		  mode="dateTime"
		  :startYear="startYear"
		  :endYear="endYear"
		  step="1"
		  @confirm="onStartTimeConfirm"
		  :defaultVal="defaultTime"
		></w-picker>
		<w-picker
		  ref="endTimePicker"
		  mode="dateTime"
		  :startYear="startYear"
		  :endYear="endYear"
		  step="1"
		  @confirm="onEndTimeConfirm"
		  :defaultVal="defaultTime"
		></w-picker>
	</view>
</template>

<script>
	import dayjs from "dayjs";
	import { uploadFile } from '../../../common/api/file.js';
	import { USER_DETAILS,getUserInfo, } from "../../../common/common";
	import { request } from "../../../common/request";
	export default {
		computed: {
		  defaultTime() {
		    const a = dayjs();
		    return [0, a.month(), a.date() - 1, a.hour() - 1, a.minute() - 1, 0];
		  },
		  startYear() {
		    return dayjs()
		      .year()
		      .toString();
		  },
		  endYear() {
		    return (+this.startYear + 1).toString();
		  },
		  memberNames() {
		    return this.ToUserList.map(i => i.UserName).join(", ");
		  }
		},
		data() {
			return {
			 info:{
				Id:'',
				SiteId:"",
				UserId:'',
				UserName:'',
				OrgId:'',
				OrgName:'',
				PlanId:'',
				PropagandaUserId:'',
				PropagandaUserName:'',
				Title:'',
				Content:'',
				Longitude:'',
				Latitude:'',
				Address:'',
				StartTime:'',
				EndTime:'',
				FileUrl:'',
				RecordVideoUrl:'',
				RecordImagesUrl:'',
				Status:'',
				IsNeedLocation:'',
				CreateTime:'',
				UpdateTime:'',
				imgUrl:''
			 },
			 option:'',
			 list:'',
			 number:'',
			 id:'',
			 RecordImagesUrl:[],
			 relevance:''
			}
		},
		methods: {
			choose(){
					uni.chooseLocation({
						type: 'wgs84',
						success:(data)=> {
								this.info.Address=data.address
								this.info.IsNeedLocation=0
								this.info.Longitude=data.longitude
								this.info.Latitude=data.latitude
								}
							})
						},
			del(res) {
					this.RecordImagesUrl.splice(res, 1);
			},
			getStr(string, str) {
				var str_before = string.split(str)[0]
				var str_after = string.split(str)[1]
				return str_before
			},
			add(){
				let count = 6-this.RecordImagesUrl.length > 0 ? 6-this.RecordImagesUrl.length : 0;
				uni.chooseImage({
				count:count,
					success: async (res) => {
						 let files = res.tempFilePaths
						 console.log(res);
						if (!files.length) {
						  return
						}
						uni.showLoading({title: '上传中'})
						files.map((item,index)=>{
						uploadFile({filePath:item}).then((res)=>{
							var url=this.getStr(res.Url,"?")
							let value={
								name:res.Url,
								url:url
							}
							console.log(value)
							this.RecordImagesUrl.push(value)
							if(this.RecordImagesUrl.length == 6){
								this.showAddRecordImage = false;
							}
							})		
						})							   
						uni.hideLoading()
						uni.showToast({title: `上传成功`})
					}
				}) 	
			},
		 StartInput(e) {
		 		this.info.StartTime=e.target.value
		 		},
		EndInput(e) {
				this.info.EndTime=e.target.value
				},
		 onInput(e) {
			 this.info.Title=e.target.value
			 },
		NameInput(e){
			this.info.UserName=e.target.value
		},
		AddressInput(e){
			this.info.Address=e.target.value
		},
		ContentInput(e){
			this.info.Content=e.target.value
		},
		 showStartTimePicker() {
		   uni.hideKeyboard();
		   this.$refs.startTimePicker.show();
		 },
		 showEndTimePicker() {
		   uni.hideKeyboard();
		   this.$refs.endTimePicker.show();
		 },
		 onStartTimeConfirm(e) {
		   if (this.info.EndTime && e.result >= this.info.EndTime) {
		     uni.showToast({
		       title: "开始时间需小于结束时间",
		       icon: "none"
		     });
		     return;
		   }
		   this.info.StartTime = e.result;
		 },
		 onEndTimeConfirm(e) {
		   if (this.info.StartTime && e.result <= this.info.StartTime) {
		     uni.showToast({
		       title: "结束时间需大于开始时间",
		       icon: "none"
		     });
		     return;
		   }
		   this.info.EndTime = e.result;
		 },
		 goTo(){
			 uni.navigateTo({
			 	url:'relevance'
			 })
		 },
		 otherFun(object){
		 if(!!object){
		 console.log(object)
		 }
		 },
		 release(){
			 if (this.RecordImagesUrl !== null) {
			 	this.info.RecordImagesUrl = JSON.stringify(this.RecordImagesUrl)
			 } else {
			 	this.info.RecordImagesUrl = []
			 }
			 //this.calculateStatus()
			 if(this.info.UserName!==getUserInfo().UserName){
				 this.info.UserId=""
			 }else{
				this.info.UserId=getUserInfo().UserId 
			 }
			 console.log(this.info)
			 if(this.number==0){
			  request({
				  url:'http://192.168.100.126:8764/propaganda/save',
				  body:this.info,
				  method:'POST'
			  }).then((res)=>{
				  uni.navigateBack(2)
			  })
			 }else if(this.number==1||this.number==2){
				request({
					url:'http://192.168.100.126:8764/propaganda/update',
					body:this.info,
					method:'POST'
				}).then((res)=>{
					uni.navigateBack(1)
				}) 
			 }
		 },
		 getDetail(id) {
		 	request({
		 		url: "http://192.168.100.126:8764/propaganda/detail",
		 		query: {
		 			recordId: id
		 		}
		 	}).then((res) => {
		 		this.list = res.Data
				if(res.Data.RecordImagesUrl==null){
					this.RecordImagesUrl=[]
				}else{
					this.RecordImagesUrl = JSON.parse(res.Data.RecordImagesUrl)
				}
		 		
		 	})
		 },
		},
		onShow(){
			uni.$on("test", (data) => {
				    this.relevance=data.test
					this.info.PropagandaUserName=data.test
			      });
		},
		mounted() {
		  this.getDetail(this.id)
		},
		onLoad(options) {
			this.id=options.id
			this.number=options.number
			this.info.OrgId=getUserInfo().OrgId
			this.info.OrgName=getUserInfo().OrgName
			this.info.UserName=getUserInfo().UserTrueName
			this.info.SiteId=getUserInfo().SiteId
			this.info.UserId=getUserInfo().UserId
			this.info.PropagandaUserName=getUserInfo().UserTrueName
			if(options.number==0){
				this.info.PlanId=options.id
				this.info.Status=1
			}else if(options.number==1||options.number==2){
			 request({
				 url:"http://192.168.100.126:8764/propaganda/detail",
				 query:{
					recordId:options.id 
				 }
			 }).then((res)=>{
				 this.info=res.Data
			 })
			}else{
			}
		}
	}
</script>

<style lang="less" scoped>
.preach-admin-release{
	padding: 1upx;
	width:100%;
	height:100%;
	
	>.top-box{
		width:100%;
		height: 150upx;
		background: #FFFFFF;
		padding: 1upx;
		
		>.top-select{
			width:50%;
			height:100%;
			float: left;
			position: relative;
			
			>.text1{
              color: #333333;
			}
			
			>.text2{		
             color: #666666;
			}
			
			>.wenhao{
			 display: block;
			 width:29upx;
			 height:29upx;
			 background-color: rgba(233, 70, 61, 1);
			 color: #FFFFFF;
			 text-align: center;
			 border-radius: 50%;
			 position: absolute;
			 right:65upx;
			 top:26upx;
			}
		}
		
		>.top-author{
			width: 50%;
			height:100%;
			float: right;
			
			>.text3{
			   font-weight: bold;
               color: #333333;
			   
			   >.text4{
				   font-size:28upx;
				   line-height: 29upx;
			   }
			}
		}
	}
	
	>.theme{
		width:100%;
		height: 140upx;
		text-align: center;
		padding: 1upx;
		
		>.theme-title{
	     margin:25upx 0;
		 font-size: 28upx;
		 font-family: PingFang SC;
		 font-weight: 500;
		 color: #333333;
		}
		
		>.theme-content{
		
		}
	}
	>.address{
		width:100%;
		height: 140upx;
		text-align: center;
		padding: 1upx;
		position: relative;
		
		>.address-title{
			margin:25upx 0;
			font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			}
			
		>.address-img{
			width:50upx;
			height: 50upx;
			position: absolute;
			right:35upx;
			top:70upx;
			
		}
	}
	>.time{
		width:100%;
		height: 140upx;
		padding:1upx;
		
		>.time-left{
		 width:50%;
		 height: 100%;
		 float:left;
		text-align: center;
		}
		
		>.time-right{
		 width:50%;
		 height: 100%;
		 float:right;
		 text-align: center;
		 
		
		}
	}
	>.preach-content{
		width:100%;
		height:80upx;
		position: relative;
		
		>.preach-content-text{
			line-height: 80upx;
			margin-left: 32upx;
			font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
		}
		
		>.preach-content-img{
			width:45upx;
			height:45upx;
			position: absolute;
			right:35upx;
			top:19upx;
		}
		
		>.icon{
			float:right;
			font-size: 28upx;
			margin-right: 20upx;
		}
	}
	>.update{
		 width:100%;
		 height: 500upx;
		 position: relative;
		
		>.count{
			position: absolute;
			right:32upx;
			top:147upx;
			font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
		}
		
		>.update-cue{
			font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
			margin: 29upx 0 0 32upx;
		}
		
		>.add{
			width: 57upx;
			height: 57upx;
			color: rgba(153, 153, 153, 1);
			transition: color .25s;
			position: absolute;
			bottom: 113upx;
			left:11upx;
		}
		>.add::before {
			content: '';
			position: absolute;
			left:59upx;
			top: 55upx;
			width: 57upx;
			margin-left: -20upx;
			margin-top: -5upx;
			border-top: 7upx solid;
		
		}
		
		>.add::after {
			content: '';
			position: absolute;
			left: 68upx;
			top: 44upx;
			height: 57upx;
			margin-left: -5upx;
			margin-top: -20upx;
			border-left: 7upx solid;
		}
		
		>.img-box {
			width: 100%;
			height: 100%;
			margin-top: 180upx;
			margin-bottom: 20upx;
		
			>.img1 {
				width: 30%;
				height: 215upx;
				float: left;
				margin: -10upx 0 20upx 18upx;
		
				>.img{
					width: 100%;
					height: 215upx;
				}
			}
		}
		
		>.position{
			width: 100%;
			height: 10upx;
		}
	}
	
	
	
	>.bottom{
	width:100%;
	height:98upx;
	position: fixed;
	left: 0;
	bottom: 0;
	padding: -1upx;
	background-color: #FFFFFF;
	 
	   >.bottom-button{
		width: 90%;
		height: 72upx;
		background: #E9463D;
		border-radius: 36upx;
		margin: 10upx auto;
		font-size: 30upx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		line-height: 72upx;
		text-align: center;
		}
	}
}

//公共样式
.text{
	display:block;
	text-align: center;
	margin:25upx;  
    font-size: 28upx;
    font-family: PingFang SC;
    font-weight:500;
}
.content{
            font-size: 28upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
       }
</style>
