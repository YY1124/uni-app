<template>
	<view class="preach-index">
		<view @click="click(item)" class="top" v-for="(item,Id) in list" :key="Id">
			<view class="top-title">
			   {{item.Title}}
			</view>
			<view class="top-date">
				计划时间：{{item.StartTime.substring(0,4)}}年{{item.StartTime.substring(5,7)}}月{{item.StartTime.substring(8,10)}}日-{{item.EndTime.substring(0,4)}}年{{item.EndTime.substring(5,7)}}月{{item.EndTime.substring(8,10)}}日
			</view>
		</view>
	</view>
</template>

<script>
	import { USER_DETAILS,getUserInfo } from "../../common/common";
	import { request } from "../../common/request";
	export default {
		data() {
			return {
			list:[],   //数据留白
			pageIndex:1,
			pageSize:10,
			identity:1,
			planId:'',
			page:10,
			loadingType:0
			}
		},
		methods: {
		click(key){
			if((getUserInfo().UserType)==this.identity){
				uni.navigateTo({
					url:'preach-admin/service?UserId='+getUserInfo().UserId+'&Id='+key.Id+'&PlanId='+this.planId+'&title='+key.Title
					})
			}else{
				uni.navigateTo({
					url:"preach-user/service?UserId="+getUserInfo().UserId+'&Id='+key.Id+'&PlanId='+this.planId+'&title='+key.Title
					})
			} 
		},
		getPreach(){
		  request({
		  	url:'http://192.168.100.126:8764/propaganda/paln/select',
			query:{
				planId:this.planId,
				pageIndex:this.pageIndex,
				pageSize:this.pageSize
			},
		  }).then((res)=>{
			 console.log(res)
			 this.list=res.Data.filter((item)=>{
			   if(item.StartTime!=null&&item.EndTime!=null){
				  return item
				 }  
			 })
		  })
		},
		getDetailAgain(Id,page){
			request({
				url:'http://192.168.100.126:8764/propaganda/paln/select',
				query:{
					planId:Id,
					pageIndex:this.pageIndex,
					pageSize:page
				}
			}).then((res)=>{
				console.log(res)
				if (this.list.length==res.Data.length) {//没有数据
								this.loadingType = 2;
								uni.hideNavigationBarLoading();//关闭加载动画
								return false;
								uni.showToast({
									title: `没有更多了`
								})
							}
							for(var i=this.list.length;i<res.Data.length;i++){
								this.list = this.list.concat(res.Data[i-1]);//将数据拼接在一起
							}
							this.loadingType = 0;//将loadingType归0重置
							uni.hideNavigationBarLoading();//关闭加载动画
			})
		},
	  },
	  mounted() {
	  	this.getPreach()
	  },
	    onPullDownRefresh() {
			  this.page=10
			  this.getPreach()
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
		   	this.getDetailAgain(this.planId,this.page)
		  }
	}
</script>

<style lang="less" scoped>
.preach-index{
	width:750upx;
	height:1205upx;
	padding: 1px;
	
	>.top{
		width:686upx;
		height:197upx;
		background-color: #FFFFFF;
		box-shadow: 0upx 4upx 20upx 0upx rgba(174,185,190,0.4);
		border-radius: 8upx;
		margin:29upx 32upx 32upx 32upx;
		padding: 1px;
		
		>.top-title{
			width:615upx;
			height: 76upx;
			font-size: 30upx;
			font-family: PingFang SC;
			font-weight: bold;
			line-height: 48upx;
			margin:20upx 38upx 89upx 33upx;
			padding: 1px;
		}
	    >.top-date{
			width:600upx;
			height:25upx;
			font-size: 26upx;
			font-family: PingFang SC;
			font-weight: 500;
			color:#999999;
			line-height: 48upx;
			margin: -60upx 149upx 32upx 32upx;
			
		}
	}
}
</style>
