<template>
	<view class="preach-admin-finish">
		<substance :Id="list" :time="time"></substance>
		<signed></signed>
		<declare :Id="list"></declare>
		<img-or-video :imgUrl="imgUrl" :videoUrl="videoUrl"></img-or-video>
		<view class="button">
			<view class="button-box" v-show="show">
				<view class="button-left" @click="Update">
					编辑宣讲计划
				</view>
				<view class="button-right" @click="finish">
					结束宣讲计划
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import { USER_DETAILS,getUserInfo, } from "../../../common/common";
	import substance from "../components/substance.vue"
	import { request } from "../../../common/request";
	import declare from"../components/declare.vue"
	import imgOrVideo from "../components/imgOrVideo.vue"
	import signed from "../components/signed.vue"
	export default {
		data() {
			return {
		      id:'',
			  list:'',
			  show:true,
			  time:'',
			  imgUrl:'',
			  videoUrl:''
			}
		},
		components:{
			substance,
			declare,
			imgOrVideo,
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
							 console.log(this.list)
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
						if((res.Data.RecordVideoUrl)){
							this.videoUrl=[]
						}else{
							this.videoUrl=JSON.parse(res.Data.RecordVideoUrl)
						}
						
				 })
				},
		Update(){
			uni.navigateTo({
				url:'update?id='+this.id
			})
		},
		finish(){
		  request({
			 url:'http://192.168.100.126:8764/propaganda/update',
			 query:{
				 Id:this.id,
				 Status:'3',
			 }
		  }).then((res)=>{
			  this.show=false
		  })
		}
		},
		onShow(){
		this.list=""
		this.getDetail(this.id)
		},
		mounted() {
		this.getDetail(this.id)	
		},
		onLoad(options){
		this.id=options.Id
		
		
		}
	}
</script>

<style lang="less" scoped>
.preach-admin-finish{
	width:100%;
	height:100%;
	padding:10upx;
	
	>.button{
		width: 100%;
		height: 97upx;
		position: fixed;
		bottom: 0;
		left:0;
		background-color: #FFFFFF;
		z-index: 20;
		
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
}
</style>
