<template>
	<view>
		
		<view class="pdlr4">
			<view class="flexEnd pdtb15">
				<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/user-myIntegralDetail/user-myIntegralDetail'}})">
					<view style="width: 30rpx;height: 32rpx;"><image src="../../static/images/integrall-icon1.png" mode=""></image></view>
					<view class="mgl5">查看流水</view>
				</view>
			</view>
			<view class="loginBj pr">
				<image src="../../static/images/integrall-icon.png" mode=""></image>
			</view>
			<view class="color6 mgb10">佣金积分</view>
			<view class="fs24 ftw">￥562</view>
		</view>
		
		<view class="loginCont">
			<view class="item flex mgb20">
				<view class="input">
					<input type="text" value="" placeholder="请输入提现金额" placeholder-class="placeholder">
				</view>
			</view>
			
			<view class="item submitbtn mgt15" style="padding: 0;" @click="Router.navigateTo({route:{path:'/pages/storeOrder/storeOrder'}})">
				<button class="Wbtn" type="submint">申请提现</button>
			</view>
			
			<view class="mgt15 red">目前为线下提现</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	
	page{padding-bottom: 60rpx; text-align: center;}	
	.loginBj{width:150rpx;height: 150rpx;margin: 60rpx auto 30rpx auto;}
	
	.loginCont{z-index: 2;background: #fff;padding: 100rpx 75rpx 80rpx 75rpx;box-sizing: border-box;}
	.loginCont .item{border-radius: 10rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #eee;}
	.loginCont .item .icon image{width: 36rpx;height: 36rpx;margin-right: 30rpx;}
	.loginCont .item .input{width: 100%;}
	.loginCont .item .input input{font-size: 26rpx;}
</style>
