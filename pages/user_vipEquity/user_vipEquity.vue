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
			<view class="ewmPic mgt10"><image src="../../static/images/vip-img.png" mode=""></image></view>
		</view>
		
		<view class="contBox mglr4 whiteBj flexCenter" style="height: 500rpx;">
			<view class="titBj white center">
				<view class="cont pr">
					<view class="bj"><image src="../../static/images/vip-img1.png" mode=""></image></view>
					<view class="tit pdlr4 ">升级权益介绍</view>
				</view>
			</view>
			<view class="mgt10">
				<scroll-view class="megText" scroll-y="true">
					<view class="xqInfor">
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
						<view>1、结果看来时间管理人世间工人国家认可考虑的价格来看房管局结果反馈了登记管理；</view>
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
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


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
