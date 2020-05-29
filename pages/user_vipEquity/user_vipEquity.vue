<template>
	<view>
		
		<view class="topBj"><image src="../../static/images/vip-img2.png" mode=""></image></view>
		
		<view class="contBox mglr4 whiteBj flexCenter" style="margin-top: -84rpx;">
			<view class="titBj white center">
				<view class="cont pr">
					<view class="bj"><image src="../../static/images/vip-img1.png" mode=""></image></view>
					<view class="tit pdlr4 ">推广二维码</view>
				</view>
			</view>
			<view class="ewmPic mgt10"><image :src="QrData.info&&QrData.info.url?QrData.info.url:''" mode=""></image></view>
		</view>
		
		<view class="contBox mglr4 whiteBj flexCenter" style="height: 500rpx;" v-if="type=='member'">
			<view class="titBj white center">
				<view class="cont pr">
					<view class="bj"><image src="../../static/images/vip-img1.png" mode=""></image></view>
					<view class="tit pdlr4 ">{{mainData.title}}</view>
				</view>
			</view>
			<view class="mgt10">
				<scroll-view class="megText" scroll-y="true">
					<view class="xqInfor">
						<view class="content ql-editor" style="padding:0;"
						v-html="mainData.content">
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				Router:this.$Router,
				type:'',
				mainData:{},
				QrData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.type = options.type;
			self.$Utils.loadAll(['getMainData','getQrData'], self);
		},
		
		methods: {
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: uni.getStorageSync('user_info').user_no,
					page: 'pages/payEnter/payEnter',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
					self.$Utils.finishFunc('getQrData');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['会员权益']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		},
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	page{background: #e9f3ff;}
	.topBj{width: 100%;height: 530rpx;}
	
	.contBox{padding: 30rpx 4%;height: 550rpx;border-radius: 30rpx;position: relative;z-index: 1;position: relative;box-sizing: border-box;margin-bottom: 90rpx;}
	.ewmPic{width: 350rpx;height: 350rpx;}
	
	.titBj{width: 540rpx;height: 70rpx;position: absolute;top: -38rpx;left: 50%;transform: translateX(-50%);}
	.titBj .cont{width: 100%;height: 100%;}
	.titBj .bj{width: 100%;height: 100%;position: absolute;top: 0;right: 0;bottom: 0;left: 0;}
	.titBj .tit{width: 100%;height: 100%;line-height: 70rpx;position: relative;z-index: 2;box-sizing: border-box;}
	
	.contBox .megText{height: 400rpx;}
	.xqInfor view{margin-bottom: 20rpx;}
</style>
