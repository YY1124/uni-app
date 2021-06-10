<template>
	<view class="preach-admin-relevance">
		<view class="content-box" v-for="(item,index) in list" :key="index">
			<view class="content">
				<view class="check-box">
					<view :class="{'active': rSelect.indexOf(index)!=-1}" class="checked" @tap="click(index)">
					</view>
				</view>
				<view class="content-list">
					<view class="content-title">
					{{item.title}}
					</view>
				    <view class="content-date">
				 	计划时间：{{item.date}}
				    </view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					{
					"id":0,
					'title':'组织学习贯彻党的十九大精神主题会议十九大精神主题会议',
					'date':'2011年2月3日-2021年3月10日'
				    },
				    {
					"id":1,	
				    'title':'组织学习贯彻党的十九大精神主题会议十九大精神主题会议',
				    'date':'2011年2月3日-2021年3月10日'
				    },
					{
					"id":2,
					'title':'组织学习贯彻党的十九大精神主题会议十九大精神主题会议',
					'date':'2011年2月3日-2021年3月10日'
					},
				],
				title:'',
				active:0,
				rSelect:[],
				text:''
				
			}
		},
		methods: {
			click(e){
				console.log(this.rSelect )//是数组，数组也有indexOf
						if (this.rSelect.indexOf(e) == -1) {
							this.rSelect.push(e);//选中添加到数组里
							
						} else {
							this.rSelect.splice(this.rSelect.indexOf(e), 1); //取消
						}
						this.text=this.list[this.active].title
						uni.$emit("test", {
						          test: this.text,
						        });
						uni.navigateBack(1)
						
					}
					
		}
		
	}
</script>

<style lang="less" scoped>
.preach-admin-relevance{
	width: 100%;
	height: 100%;
	padding:1upx;
	
	>.content-box{
		width:686upx;
		height: 186upx;
		background-color: #FFFFFF;
		margin: 37upx 31upx 0 33upx;
		box-shadow: 0upx 4upx 20upx 0upx rgba(174, 185, 190, 0.4);
		
		>.content{
		   width:686upx;
		   height: 186upx;
		   
		 >.check-box{
			width:15%;
			height:100%;
			float: left;
			
			
			>.checked{
				width:32upx;
				height:32upx;
				border-radius: 50%;
				border:1upx solid rgba(204, 204, 204, .8);
				margin: 77upx auto;
			}
		 }	
		 >.content-list{
			height: 100%;
			padding: 1upx;
			
			>.content-title{
				width: 90%;
				height: 70upx;
				font-size: 28upx;
				font-family: PingFang SC;
				font-weight: bold;
				color: #333333;
				line-height: 43upx;
				margin: 28upx 30upx 20upx 70upx;
			}
			
			>.content-date{
				width: 100%;
				height: 23upx;
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #999999;
				line-height: 48upx;
			}
			}
		}
		
	}
	
	
}
.defa{
}
.active{
	background-color: #DD524D;
}
</style>
