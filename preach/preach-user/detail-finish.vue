<template>
	<view class="preach-user-finish">
		<substance :Id="list" :time="time"></substance>
		<signed></signed>
		<declare :Id="list"></declare>
		<img-or-video :imgUrl="imgUrl" :videoUrl="videoUrl"></img-or-video>
	</view>
</template>

<script>
	import substance from "../components/substance.vue"
	import declare from"../components/declare.vue"
	import imgOrVideo from "../components/imgOrVideo.vue"
	import signed from "../components/signed.vue"
	import { request } from "../../../common/request";
	export default {
		data() {
			return {
			id:"",
			list:"",
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
							 		"Endhour":this.list.EndTime.substring(11,16)
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
.preach-user-finish{
	width:100%;
	height:100%;
	padding:10upx;
}
</style>
