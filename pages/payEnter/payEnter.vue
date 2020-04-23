<template>
	<view>
		<view class="flexCenter">
			<view class="topIcon"><image src="../../static/images/icon.png" mode=""></image></view>
		</view>
		<view class="fs13 myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr"><input type="text" value="" placeholder="填写姓名" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">手机号</view>
				<view class="rr"><input type="text" value="" placeholder="填写手机号" maxlength="11" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">邀请码</view>
				<view class="rr">26563</view>
			</view>
		</view>
		
		<view class="mglr4 money center">需要支付366元</view>
		<view class="submitbtn">
			<button class="btn" type="default" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">提交</button>
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 40rpx;}
	
	.topIcon{width: 310rpx;height: 301rpx;margin: 80rpx 0 40rpx 0;}
	.money{font-size: 36rpx;line-height: 60rpx;font-weight: bold; color: #e41515;padding: 100rpx 4% 120rpx 4%;}
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .item .rr{font-size: 28rpx;}
</style>
