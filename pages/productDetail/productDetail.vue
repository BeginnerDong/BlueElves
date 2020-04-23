<template>
	<view>
		
		<view class="banner-box">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#f8b734" indicator-color="rgba(173,173,173)">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mglr4 pdtb15 detailTit">
			<view class="fs15 ftw">闻达香 有机温水稻花香小米+有机绿小米400g</view>
			<view class="flex pdt20">
				<view class="price">79</view>
				<view class="priceTit">销售价</view>
				<view class="price2">￥69</view>
				<view class="priceTit">成本价</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="pdt15" >
			<view class="mglr4 xqInfor">
				<view class="fs15 ftw pdb20">商品详情描述</view>
				<view class="cont fs14 center">
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
					<view><image class="w" src="../../static/images/detailsl-img1.png" mode="widthFix"></image></view>
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar center" style="height: 100rpx;">
			<view class=" flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="icon">
						<image src="../../static/images/detailsl-icon1.png" mode=""></image>
					</view>
					<view class="mgt5">首页</view>
				</view>
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages//'}})">
					<view class="icon pr">
						<image src="../../static/images/detailsl-icon.png" mode=""></image>
						<view class="carNum">3</view>
					</view>
					<view class="mgt5">购物车</view>
				</view>
			</view>
			<view class="flexEnd">
				<view class="payBtn"style="background-color: #75b9f3;">加入购物车</view>
				<view class="payBtn" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即订购</view>
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
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/detailsl-img.png",
					"../../static/images/detailsl-img.png",
					"../../static/images/detailsl-img.png"
				],
				specsCurr:0,
				specsData:['定制版120ML','定制版100ML']
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			specsChange(index){
				const self = this;
				self.specsCurr = index
			},
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.swiper-box {height: 400rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	
	.detailTit .price{font-size: 36rpx; font-weight: bold;}
	.detailTit .price2{font-size: 24rpx; font-weight: bold; color: #666;margin-left: 20rpx;}
	.detailTit .priceTit{font-size: 22rpx; color: #999;margin-left: 10rpx;}
	
	
</style>
