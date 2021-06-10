<template>
	<view class="preach-admin-service">
		<view class="top">
			<text class="text">
				宣讲计划：{{this.options.title}}
			</text>
		</view>
		<view class="tab-box">
			<view class="tab">
				<text class="box" :style="style1" @click="click(1)">全部宣讲</text>
			</view>
			<view class="tab">
				<text class="box" :style="style2" @click="click(2)">我的宣讲</text>
			</view>
		</view>
		<view class="content-box" @click="Go(item)" v-for="item in item" :key="item.Id">
			<view class="content">
				{{item.Title}}
			</view>
			<view class="preacher">
				宣讲人：{{item.UserName}}
			</view>
			<view class="date" v-if="item.StartTime==null">
				宣讲时间：
			</view>
			<view class="date" v-else>
				宣讲时间：{{item.StartTime.substring(0,4)}}年{{item.StartTime.substring(5,7)}}月{{item.StartTime.substring(8,10)}}日  {{item.StartTime.substring(11,16)}}
			</view>
			<view v-if="(item.State)==1" style="background:#FFA200;" class="state">
				未开始
			</view>
			<view v-if="(item.State)==2" style="background:#21C499;" class="state">
				进行中
			</view>
			<view v-if="(item.State)==3" style="background:#A3A3A3;" class="state">
				已结束
			</view>
			<view v-if="(item.State)==null" style="background:#A3A3A3;" class="state">
			    未安排
			</view>
		</view>
		<view class="release" @click="goTo">
			<view class="add">

			</view>
		</view>
	</view>
</template>

<script>
	import { USER_DETAILS,getUserInfo, } from "../../../common/common";
	import { request } from "../../../common/request";
	export default {
		data() {
			return {
				item:[],
				style1: 'border-bottom:2px solid rgba(233, 70, 61, 1);',
				style2: '',
				keywords:'',
				pageIndex:1,
				pageSize:10,
				options:'',
				title:'',
				res:true,
				loadingType:0,
				page:10,
			}
		},
		methods: {
			click(res) {
				if (res === 1) {
			        this.getDetail(null,this.options.Id)
					this.style1 = "border-bottom:2px solid rgba(233, 70, 61, 1);"
					this.style2 = ""
				} else {
					this.getDetail(getUserInfo().UserId,this.options.Id)
					this.style2 = "border-bottom:2px solid rgba(233, 70, 61, 1);"
					this.style1 = ""
				}
			},
			goTo(){
				uni.navigateTo({
					url:'release?number=0&id='+this.options.Id,
				})
			},
			Go(res){
				if((res.State)==1||(res.State)==null||(res.State)==0){
				uni.navigateTo({
					url:'detail-start?Id='+res.Id+'&propaganda='+res.PropagandaUserId
				})	
				}else if((res.State)==2){
					uni.navigateTo({
						url:'detail-on?Id='+res.Id+'&propaganda='+res.PropagandaUserId
					})
				}else{
					uni.navigateTo({
						url:'detail-finish?Id='+res.Id+'&propaganda='+res.PropagandaUserId
					})
				}
				
			},
			calculateStatus(EndTime,StartTime){//计算其状态的方法
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
						 if(time<EndTime&&time>StartTime){
							 return 2
						 }else if(time>EndTime){
							return 3
						 }else if(time<EndTime&&time<StartTime){
							return 1
						 }
						 
			},
			getDetail(id,Id){
				request({
					url:"http://192.168.100.126:8764/propaganda/select",
					query:{
						userId:id,
						orgId:'',
						planId:Id,
						keywords:this.keywords,
						pageIndex:this.pageIndex,
						pageSize:this.pageSize,
					}
				}).then((res)=>{
					this.item=res.Data
					for (var i = 0; i < res.Data.length; i++) {
					res.Data[i].State=this.calculateStatus(res.Data[i].EndTime,res.Data[i].StartTime)
					}
					console.log(res.Data)
				})
				
			},
			getDetailAgain(Id,page){
				request({
					url:"http://192.168.100.126:8764/propaganda/select",
					query:{
						userId:'',
						orgId:'',
						planId:Id,
						keywords:this.keywords,
						pageIndex:this.pageIndex,
						pageSize:page,
					}
				}).then((res)=>{
					console.log(res)
					if (this.item.length==res.Data.length) {//没有数据
									this.loadingType = 2;
									uni.hideNavigationBarLoading();//关闭加载动画
									return false;
									uni.showToast({
										title: `没有更多了`
									})
								}
								for(var i=this.item.length;i<res.Data.length;i++){
									this.item = this.item.concat(res.Data[i-1]);//将数据拼接在一起
								}
								this.loadingType = 0;//将loadingType归0重置
								uni.hideNavigationBarLoading();//关闭加载动画
				})
			},
			
		},
		onShow(){
			this.getDetail(null,this.options.Id)
		},
		mounted() {
			this.getDetail(null,this.options.Id)
		},
		onLoad(options) {
			    this.options=options
		       },
		onPullDownRefresh() {
			      this.page=10
		   		   this.getDetail(this.options.Id)
		           setTimeout(function () {
		               uni.stopPullDownRefresh();
					   uni.showToast({
					   	title: `刷新完毕`
					   })
		           }, 1000);
		  },
		  
		  // 上拉加载
		  onReachBottom: function() {
		  	this.page+=5//每触底一次 pageSize +1
			if (this.loadingType != 0) {//loadingType!=0;直接返回
					return false;
				}
			this.loadingType = 1;
		  	uni.showNavigationBarLoading()//显示加载动画
		  	this.getDetailAgain(this.options.Id,this.page)
		 }
	}
</script>

<style lang="less" scoped>
	.preach-admin-service {
		padding: 1upx;
		width: 750upx;
		height: 1205upx;

		>.top {
			width: 721upx;
			height: 72upx;
			background: linear-gradient(to right, rgba(255, 77, 77, .2), rgba(255, 77, 77, .2), #FFFFFF);
			line-height: 72upx;
			padding: 1px;

			>.text {
				font-size: 28upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #666666;
				padding-left: 32upx;
				line-height: 48upx;
			}
		}

		>.tab-box {
			width: 750upx;
			height: 88upx;
			background: #FFFFFF;

			>.tab {
				float: left;
				width: 50%;
				height: 100%;
				text-align: center;

				>.box {
					width: 30%;
					height: 88upx;
					line-height: 88upx;
					text-align: center;
					display: inline-block;

				}
			}
		}

		>.content-box {
			width: 686upx;
			height: 240upx;
			background: #FFFFFF;
			box-shadow: 0upx 4upx 20upx 0upx rgba(174, 185, 190, 0.4);
			border-radius: 8upx;
			margin: 34upx 32upx 32upx;
			padding: 1upx;
			position: relative;

			>.content {
				width: 599upx;
				height: 78upx;
				font-size: 30upx;
				font-family: PingFang SC;
				font-weight: bold;
				color: #333333;
				line-height: 48upx;
				margin: 32upx 54upx 130upx 33upx;

			}

			>.preacher {
				width: 100%;
				height: 23upx;
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #999999;
				line-height: 48upx;
				margin: -100upx 0 20upx 33upx;
			}

			>.date {
				width: 100%;
				height: 23upx;
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #999999;
				line-height: 48upx;
				margin: 0upx 0 20upx 33upx;
			}

			>.state {
				position: absolute;
				bottom: 30upx;
				right: 20upx;
				width: 100upx;
				height: 40upx;
				background: #FFA200;
				border-radius: 21upx 20upx 20upx 0upx;
				line-height: 40upx;
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #FFFFFF;
				text-align: center;
			}
		}

		>.release {
			width: 112upx;
			height: 112upx;
			background: #E9463D;
			border-radius: 50%;
			position: fixed;
			bottom: 69upx;
			right: 32upx;
			padding: 1upx;

			>.add {
				width: 50upx;
				height: 50upx;
				color: #FFFFFF;
				transition: color .25s;
				position: relative;
				z-index: 20;
			}

			>.add::before {
				content: '';
				position: absolute;
				left:52upx;
				top: 55upx;
				width: 45upx;
				margin-left: -20upx;
				margin-top: -5upx;
				border-top: 7upx solid;

			}

			>.add::after {
				content: '';
				position: absolute;
				left: 55upx;
				top: 50upx;
				height: 45upx;
				margin-left: -5upx;
				margin-top: -20upx;
				border-left: 7upx solid;
			}
		}
	}
</style>
