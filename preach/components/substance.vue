<template>
	<view class="substance">
		<view class="detail">
			<view class="detail-top">
				{{Id.Title}}
			</view>
			<view class="detail-text">
				<text class="text-title">宣讲人</text> 
				<text class="text-content">{{Id.UserName}}</text>
			</view>
			<view class="detail-text">
				<text class="text-title">宣讲时间</text>
				<view class="text-content" v-if="(this.Id.StartTime)!==null||(this.Id.EndTime)!==null">{{time.year}}年{{time.month}}月{{time.day}}日 {{time.week}} {{time.Starthour}}-{{time.Endhour}}</view>
			    <text class="text-content" v-else></text>
			</view>
			<view class="detail-text">
				<text class="text-title">宣讲地点</text>
				<text class="text-content">{{Id.Address}}</text>
				<image class="img" src="../../../static/preach/dingwei1.png" mode=""></image>
			</view>
			<view class="detail-text">
				<text class="text-title">宣讲计划</text>
				<text class="text-content">{{Id.PlanName}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:['Id','time'],
		data() {
			return {
			}
		},
		methods: {
		}
	}
</script>

<style lang="less" scoped>
.substance{
	padding: 1upx;
	width: 100%;
	height: 100%;
	
	>.detail{
		width:100%;
		height: 595upx;
		padding: 1upx;
		
		>.detail-top{
			width:675upx;
			height:78upx;
			font-size: 30upx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #333333;
			line-height: 48upx;
			margin: 32upx 43upx 50upx 36upx;
		}
		
		>.detail-text{
			width: 100%;
			height: 73upx;
			font-size: 26upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #666666;
			margin: 36upx 0 0 37upx;
			position:relative ;
			
			>.text-title{
			  display: block;
			  margin-bottom:15upx ;
			  font-size: 26upx;
			  font-weight: 500;
	          font-family: PingFang SC;
			  color:rgba(102, 102, 102, 1);
			}
			>.text-content{
			  font-size: 28upx;
			  font-weight: bold;
			  font-family: PingFang SC;
			  color:rgba(51, 51, 51, 1);
			}
			
			>.img{
				width:50upx;
				height:55upx;
				float: right;
				margin: -10upx 80upx 0 0;
			}
		}
	}
	
}
</style>
