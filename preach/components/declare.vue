<template>
	<view class="declare">
	 <view class="content">
		<view class="content-title">
			<text class="content-text">宣讲内容</text>
		</view>
		<view class="content-artcle">
			{{Id.Content}}
		</view>
	 </view>	
	</view>
</template>

<script>
	export default {
		props:["Id"],
		data() {
			return {
			}
		},
		methods: {
	  },
	}
</script>

<style lang="less" scoped>
.declare{
   padding:1upx;
   width: 100%;
   height:100%;
   margin-bottom: 60upx;
   
   
   >.content{
   	width: 100%;
   	height: 100%;
   	
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
   	 min-height: 150upx;
   	 font-size: 22upx;
   	 font-family: PingFang SC;
   	 font-weight: 500;
   	 color: #666666;
   	 line-height: 54upx; 
   	 margin-left: 32upx;
   	 z-index: 19;
     }
   }
}
</style>
