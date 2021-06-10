<template>
	<view class="imgOrVideo">
		<view class="content">
				<view class="content-title">
					<text class="content-text">宣讲视频/照片</text>
				</view>
				<view class="content-artcle">
					<view class="video-left"  v-for="(item,index) in this.imgUrl" :key="index">
						<image :style="style" :src="item.url" mode=""></image>
					</view>
					<view class="video-left"  v-for="(item,index) in this.videoUrl" :key="index">
						<video  :style="style" :src="item.url" controls></video>
						
					</view>
				</view>
		</view>	
	</view>
</template>

<script>
	export default {
		props:['imgUrl','videoUrl'],
		data() {
			return {
			style:"width: 100%;height:152upx;",
			video:false,
			img:false,
			}
		},
		methods: {
		}
	}
</script>

<style lang="less" scoped>
.imgOrVideo{
	width:100%;
	height: 100%;
	padding:1upx;
	
	>.content{
		width: 100%;
		height: 290upx;
		margin-top:-90upx ;
		margin-bottom: 80upx;
		
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
		padding: 1upx;
		height:152upx;
		width:90%;
		margin:38upx auto;
		
		>.video-left{
			width:31%;
			height: 100%;
			float:left;
			margin: 10upx 13upx 0 0;
			background-color: #D4B579;
		}
		
		>.video-middle{
			width:31%;
			height: 100%;
			float:left;
			margin: 10upx 13upx 0 0;
			background-color: #D4B579;
		}
		
		>.video-right{
			width:31%;
			height: 100%;
			float:left;
			margin: 10upx 13upx 0 0;
			background-color: #D4B579;
		}
		
	  }
	}
}
</style>
