<template>
	<view class="preach-admin-update">
		<view class="top">
			<text class="text">宣讲视频</text>
			<text class="text1">(最多上传6个视频)</text>
		</view>
		<view class="middle">
			<view class="add-box">
				<view class="add" v-model="video" @click="add(video)">
				</view>

			</view>
			<view class="video-box">
				<view class="video1" @click="del(index,1)" v-for="(item,index) in RecordVideoUrl" :key="index">
					<video class="video" :src="item.name"></video>
				</view>
			</view>
		</view>
		<view class="top">
			<text class="text">宣讲照片</text>
			<text class="text1">(最多上传9张照片)</text>
		</view>
		<view class="middle">
			<view class="add-box">
				<view class="add" v-model="image" @click="add(image)">
				</view>

			</view>
			<view class="video-box">
				<view class="video1" @click="del(index,2)" v-for="(item,index) in RecordImagesUrl" :key="index">
					<image class="video" :src="item.name"></image>
				</view>
			</view>
		</view>
		<view class="bottom" @click="submit">
			<view class="bottom-button">
				发布
			</view>
		</view>
	</view>
</template>

<script>
	import {uploadFile} from '../../../common/api/file.js';
	import {request} from "../../../common/request";
	import {BASE_IMAGE_UPLOAD_URL,getUserInfo,} from "../../../common/common";
	export default {
		data() {
			return {
				video: "video",
				image: "image",
				RecordImagesUrl: [],
				RecordVideoUrl: [],
				showAddImage: true,
				showAddRecordImage: true,
				showAddRecordVideo: true,
				ImgsUrl: [],
				list: "",
				id: ''
			}
		},
		methods: {
			getDetail(id) {
				request({
					url: "http://192.168.100.126:8764/propaganda/detail",
					query: {
						recordId: id
					}
				}).then((res) => {
					this.list = res.Data
					this.RecordImagesUrl = JSON.parse(res.Data.RecordImagesUrl)
					this.RecordVideoUrl = JSON.parse(res.Data.RecordVideoUrl)
					console.log(this.RecordVideoUrl)
				})
			},
			submit() {
				if (this.RecordImagesUrl !== null) {
					this.list.RecordImagesUrl = JSON.stringify(this.RecordImagesUrl)
				} else {
					this.list.RecordImagesUrl = []
				}
				if (this.RecordVideoUrl !== null) {
					this.list.RecordVideoUrl = JSON.stringify(this.RecordVideoUrl)
					
				} else {
					this.list.RecordVideoUrl = []
				}
				request({
					url: 'http://192.168.100.126:8764/propaganda/update',
					body: this.list,
					method: 'POST'
				}).then((res) => {
					uni.navigateBack(1)
				})

			},
			del(res, number) {
				if (number == 1) {
					this.RecordVideoUrl.splice(res, 1);
				} else if (number == 2) {
					this.RecordImagesUrl.splice(res, 1);
				}
			},
			add(res) {
				if (this.RecordVideoUrl.length >= 6) {
					uni.showToast({
						title: "最多允许上传6个视频",
						icon: "none"
					});
					return;
				}
				if (this.RecordImagesUrl.length >= 9) {
					uni.showToast({
						title: "最多允许上传9张图片",
						icon: "none"
					});
					return;
				}
				let count = 9 - this.RecordImagesUrl.length > 0 ? 9 - this.RecordImagesUrl.length : 0;
				let countVideo = 6 - this.RecordVideoUrl.length > 0 ? 6 - this.RecordVideoUrl.length : 0;
				if (res == 'image') {
					uni.chooseImage({
						count: count,
						success: async (res) => {
							let files = res.tempFilePaths
							if (!files.length) {
								return
							}
							uni.showLoading({
								title: '上传中'
							})
							files.map((item, index) => {
								uploadFile({
									filePath: item
								}).then((res) => {
									var url = this.getStr(res.Url, "?")
									let value = {
										name: res.Url,
										url: url
									}
									this.RecordImagesUrl.push(value)
									if (this.RecordImagesUrl.length == 9) {
										this.showAddRecordImage = false;
									}
								})
							})
							uni.hideLoading()
							uni.showToast({
								title: `上传成功`
							})
						}
					})
				} else if (res == 'video') {
					uni.chooseVideo({
						count: countVideo,
						sourceType: ['camera', 'album'],
						success:  async (res) =>  {
								let files = res.tempFilePath;
									uploadFile({
										filePath: res.tempFilePath
										}).then((res) => {
											var url = this.getStr(res.Url, "?")
											let value = {
												name: res.Url,
												url: url
											}
											this.RecordVideoUrl.push(value)
											if (this.RecordVideoUrl.length == 6) {
												this.showAddRecordVideo= false;
											}
										})
									
							uni.hideLoading()
							uni.showToast({title: `上传成功`})
					     }
					})
				}
			},
			getStr(string, str) {
				var str_before = string.split(str)[0]
				var str_after = string.split(str)[1]
				return str_before
			}
		},
		mounted() {
			this.getDetail(this.id)
		},
		onLoad(options) {
			this.id = options.id
		}
	}
</script>

<style lang="less" scoped>
	.preach-admin-update {
		width: 100%;
		height: 100%;
		padding: 1upx;
		padding-bottom: 150upx;

		>.top {
			width: 100%;
			height: 100upx;
			line-height: 80upx;
			padding-left: 32upx;

			>.text {
				font-size: 28upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: rgba(51, 51, 51, 1);
			}

			>.text1 {
				font-size: 28upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: rgba(153, 153, 153, 1);
			}
		}

		>.middle {
			width: 100%;
			height: 100%;
			padding: 1upx;

			>.video-box {
				height: 100%;
				height: 215upx;
				margin-top: -50upx;
				margin-bottom: 250upx;

				>.video1 {
					width: 30%;
					height: 215upx;
					float: left;
					margin: -10upx 0 20upx 18upx;

					>.video {
						width: 100%;
						height: 215upx;
					}
				}
			}

			>.add-box {
				width: 30%;
				height: 215upx;
				margin: 20upx 0upx 0upx 32upx;

				>.add {
					width: 57upx;
					height: 215upx;
					position: relative;

					color: rgba(153, 153, 153, 1);
				}

				>.add::before {
					content: '';
					position: absolute;
					left: 59upx;
					top: 55upx;
					width: 57upx;
					margin-left: -20upx;
					margin-top: -5upx;
					border-top: 7upx solid;
					border-radius: 6upx;
				}

				>.add::after {
					content: '';
					position: absolute;
					left: 68upx;
					top: 44upx;
					height: 57upx;
					margin-left: -5upx;
					margin-top: -20upx;
					border-left: 7upx solid;
					border-radius: 6upx;
				}
			}
			
		}

		>.bottom {
			width: 100%;
			height: 98upx;
			position: fixed;
			left: 0;
			bottom: 0;
			padding: 1upx;
			z-index: 20;

			>.bottom-button {
				width: 90%;
				height: 72upx;
				background: #E9463D;
				border-radius: 36upx;
				margin: 10upx auto;
				font-size: 30upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #FFFFFF;
				line-height: 72upx;
				text-align: center;
			}
		}
	}
</style>
