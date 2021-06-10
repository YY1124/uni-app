<template>
	<view class="sign-box">
		<navigator url="../sign" class="sign">
			<text class="sign-title">签到情况统计</text>
			<image class="sign-more" src="../../../static/preach/jiantou.png"> </image>
		</navigator>
	</view>
</template>

<script>
	import { USER_DETAILS,getUserInfo, } from "../../../common/common";
	import { request } from "../../../common/request";
	export default {
		data() {
			return {
				url:'http://192.168.100.126:8764/propagandaSign/signup/select'
			}
		},
		methods: {
			/* getSigned(){
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
					console.log(res)
				})
			} */
		},
		mounted() {
			//this.getSigned()
		}
	}
</script>

<style lang="less" scoped>
.sign-box{
	
	>.sign{
		width: 100%;
		height:88upx;
		margin:15upx 0  -5upx 0;
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
}
</style>
