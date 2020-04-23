<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<scroll-view scroll-x="true" >
					<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">全部</view>
					<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">待处理</view>
					<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">待发货</view>
					<view class="tt" :class="curr==4?'on':''" @click="currChange('4')">已发货</view>
					<view class="tt" :class="curr==5?'on':''" @click="currChange('5')">已完成</view>
					<view class="tt" :class="curr==6?'on':''" @click="currChange('6')">已退货</view>
				</scroll-view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red" v-if="curr==1">已发货</view>
							<view class="red" v-if="curr==2">待处理</view>
							<view class="red" v-if="curr==3">待发货</view>
							<view class="red" v-if="curr==4">已发货</view>
							<view class="red" v-if="curr==5">已完成</view>
							<view class="red" v-if="curr==6">待发货</view>
						</view>
						<view class="flexRowBetween">
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
						<view class="mgt15 flexEnd underBtn" v-if="curr==1">
							<view class="Bbtn">确认收货</view>
						</view>
						<view class="mgt15 flexEnd underBtn" v-if="curr==2">
							<view class="Bbtn blue">去支付</view>
						</view>
						<view class="mgt15 flexEnd underBtn" v-if="curr==3">
							<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/userVIP-returnGoods/userVIP-returnGoods'}})">申请退货</view>
						</view>
						<view class="mgt15 flexEnd underBtn" v-if="curr==4">
							<view class="Bbtn">确认收货</view>
						</view>
						<view class="adviseF5 flexRowBetween f5bj fs13 mgt15" v-if="curr==6">
							<view class="ftw">退货原因</view>
							<view class="text pdl10 color6">会计法克里斯居里夫人食挂号费圣诞节快乐和规范挂号费接电话</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 80rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	
	.orderNav scroll-view{white-space: nowrap;}
	.orderNav scroll-view .tt{width: auto;margin: 0 30rpx;display: inline-block;}
	
	.proRow .item{margin-top: 30rpx;padding-top: 20rpx;}
	.proRow .item .infor{width: 68%;}
	
	.adviseF5{align-items: flex-start;padding: 20rpx;}
	.adviseF5 .text{width: 81%;}
</style>
