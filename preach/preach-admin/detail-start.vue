<template>
	<view class="preach-admin-detail-start">
		<substance :Id="list" :time="time"></substance>
		<signed></signed>
		<declare :Id="list"></declare>
		<view class="button">
			<view class="button-box">
				<view class="button-left" @click="edit">
					编辑
				</view>
				<view class="button-right" @click="del">
					删除
				</view>
			</view>
		</view>
		<view class="del-box" v-show="show">
			<view class="del-text">你确定要将其删除？</view>
			<view class="right" @click="right">
				确定
			</view>
			<view class="fault" @click="fault">
				取消
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
				id:"",
				list:'',
				show:false,
				time:''
			}
		},
		components:{
			substance,
			declare,
			signed
		},
		methods: {
		edit(){
			uni.navigateTo({
				url:'release?number=1&id='+this.id
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
		 del(){
			this.show=true 
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
		 right(){
			request({
				url:"http://192.168.100.126:8764/propaganda/delete",
				query:{
					propaganda:this.id
				}
			}).then((res)=>{
				this.show=false
				uni.navigateBack(1)
			}) 
		 },
		 fault(){
	       this.show=false 
		 }
		},
		mounted() {
			this.getDetail(this.id)
		},
		onShow() {
			this.list=""
			this.getDetail(this.id)
		},
		onLoad(options) {
			this.id=options.Id
		},
	}
</script>

<style lang="less" scoped>
.preach-admin-detail-start{
	width:100%;
	height: 100%;
    padding: 1upx;
	
	>.content{
		width: 100%;
		height: 489upx;
		
		>.content-title{
			width: 100%;
			height: 31upx;
			font-size: 32upx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #333333;
			line-height: 74upx;
			margin: 34upx 0 40upx 55upx;
			position: relative;
			
			>.content-text::before{
				content: "";
				height:28upx;
				width: 6upx;
				background-color: rgba(233, 70, 61, 1);
				position: absolute;
				left:-18upx;
				top:25upx;	
			}
		}
	  >.content-artcle{
		 width: 670upx;
		 height: 351upx;
		 font-size: 22upx;
		 font-family: PingFang SC;
		 font-weight: 500;
		 color: #666666;
		 line-height: 54upx; 
		 margin-left: 32upx;
	  }
	}
	
	>.button{
		width: 100%;
		height: 97upx;
		position: fixed;
		bottom: 0;
		left:0;
		background-color: #FFFFFF;
		
		>.button-box{
			width:672upx;
			height: 72upx;
			margin: 13upx auto;
			
			>.button-left{
			 float: left;
			 width: 50%;
			 height: 100%;
			 line-height: 72upx;
			 text-align: center;
			 background: rgba(255, 162, 0, 1);
			 font-size: 30upx;
			 font-family: PingFang SC;
			 font-weight: 500;
			 color: #FFFFFF;
			 border-radius: 36upx 0upx 0upx 36upx;
			 
			}
			>.button-right{
			float:right;
			width: 50%;
			height: 100%;
			line-height: 72upx;
			text-align: center;
			background: rgba(233, 70, 61, 1);
			font-size: 30upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #FFFFFF;
			border-radius: 0upx 36upx 36upx 0upx;
			}
		}
		
	}
	
	>.del-box{
	  width:100%;
	  height:250upx;
	  background-color: #FFFFFF;
	  position: fixed;
	  bottom:0;
	  left:0;
	  border-radius: 5upx;
      box-shadow: 0px 4px 20px 0px rgba(174, 185, 190, 0.4);
	  
	  >.del-text{
		  width:100%;
		  height: 150upx;
		  line-height:150upx ;
		  text-align: center;
		  font-size: 30upx;
		  font-family: PingFang SC;
		  font-weight: 500;
	  }
	  
	  >.right{
		 float: left;
		 width: 50%;
		 text-align: center;
		 height:50upx;
		 line-height: 50upx;
		 font-family: PingFang SC;
		 font-weight: 500;
	  }
	  
	  >.fault{
		float: right;
		width: 50%;
		text-align: center;
		height:50upx;
		line-height: 50upx;
		font-family: PingFang SC;
		font-weight: 500;
	  }
	}
}
</style>
