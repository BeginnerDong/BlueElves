<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">待处理</view>
				<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">待发货</view>
				<view class="tt" :class="curr==4?'on':''" @click="currChange('4')">已发货</view>
				<view class="tt" :class="curr==5?'on':''" @click="currChange('5')">已完成</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		
		<view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待处理</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">79</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥69</view>
										<view class="priceTit">成本价</view>
									</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待发货</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">79</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥69</view>
										<view class="priceTit">成本价</view>
									</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已发货</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">79</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥69</view>
										<view class="priceTit">成本价</view>
									</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="mgt15 flexEnd underBtn">
							<view class="Bbtn">确认收货</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已完成</view>
						</view>
						<view class=" flexRowBetween">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">79</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥69</view>
										<view class="priceTit">成本价</view>
									</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" src="../../static/images/the-orderl-img.png" mode=""></image></view>
			<view class="closeBtn" @click="hxEwmShow">×</view>
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
				wx_info:{},
				is_show:false,
				curr:1,
				is_hxEwmShow:false,
				proData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	/* @import "../../assets/style/editInfor.css"; */
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 80rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	
	.proRow .item{margin-top: 30rpx;padding-top: 20rpx;}
	.proRow .item .infor{width: 68%;}
	.proList{padding-bottom: 20rpx;}
	.proList:last-child{padding-bottom: 0;}
	
	.hxEwm{width: 85rpx;height: 85rpx;}
	
	.hxEwmShow{width: 70%;position: fixed;top: 45%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.hxEwmShow .ewm{width: 400rpx;height: 400rpx;display: block; margin: 0 auto;}
	.closeBtn{background: rgba(0,0,0,0.5);border: 0;top: auto;bottom: -130rpx;}
</style>
