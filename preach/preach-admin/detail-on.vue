<template>
	<view class="preach-admin-detail-on">
		<substance :Id="list" :time="time"></substance>
		<view class="sign" @click="click">
			<text class="sign-title">签到二维码</text>
			<image class="sign-more" src="../../../static/preach/jiantou.png"></image>
		</view>
		<signed></signed>
		<declare :Id="list"></declare>
		<view class="button" @click="on">
			<view class="button-box">
					上传宣讲记录
		</view>
		</view>
	</view>
</template>

<script>
	import substance from"../components/substance.vue"
	import declare from"../components/declare.vue"
	import signed from "../components/signed.vue"
	import { request } from "../../../common/request";
	export default {
		data() {
			return {
				id:'',
				list:'',
				time:''
			}
		},
		components:{
			substance,
			declare,
			signed
		},
		methods: {
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
							 this.list=res.Data
							 this.time={
							 		"year":this.list.StartTime.substring(0,4),
							 		"month":this.list.StartTime.substring(5,7),
							 		"day":this.list.StartTime.substring(8,10),
							 		"Starthour":this.list.StartTime.substring(11,16),
							 		"Endhour":this.list.EndTime.substring(11,16),
									"week":this.Week(this.list.StartTime.substring(0,4),this.list.StartTime.substring(5,7),this.list.StartTime.substring(8,10),)
							 	}
				 })
		},
		click(){
		   uni.navigateTo({
		   	url:'../QR'
		   })
		},
		on(){
		   uni.navigateTo({
		   	url:'release?number=2&id='+this.id
		   })
		},
		
		},
		mounted() {
		this.getDetail(this.id)
		},
		onShow(){
		  this.list=""
		  this.getDetail(this.id)
		},
		onLoad(options) {
			this.id=options.Id
		}
	}
</script>

<style lang="less" scoped>
.preach-admin-detail-on{
	width:100%;
	height: 100%;
    padding: 1upx;
	
	>.sign{
		width: 100%;
		height:88upx;
		margin:20upx 0 -10upx 0;
		position: relative;
		
		>.sign-title{
	      margin-left: 38upx;
		  line-height: 88upx;
		  font-size: 30upx;
		  font-family: PingFang SC;
		  font-weight: bold;
		  color: #333333;
		}
		>.sign-more{
		  width:30upx;
		  height:30upx;
		  position: absolute;
		  right:30upx;
		  top:35upx;
		}
		
	}
	>.button{
		width: 100%;
		height: 97upx;
		position: fixed;
		bottom: 0;
		left:0;
		z-index: 20;
		background-color: #FFFFFF;
		
		>.button-box{
			width:672upx;
			height: 72upx;
			margin: 13upx auto;
			line-height: 72upx;
			text-align: center;
			background: rgba(233, 70, 61, 1);
			font-size: 30upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #FFFFFF;
			border-radius:36upx;
		
		}
		
	}
}
.two{
	margin-top: -20upx;
}
</style>
