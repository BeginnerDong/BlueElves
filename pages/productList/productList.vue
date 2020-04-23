<template>
	<view>
		
		<view class="">
			<scroll-view scroll-x class="orderNav whiteBj color6 boxShaow" >
				<view class="tt" v-for="(item,index) in navList" :key="index" :class="curr==index?'on':''" @click="changeCurr(index)">{{item}}</view>
			</scroll-view>
		</view>
		<view class="orderNavH"></view>
		<view class="mglr4 productList mgt10">
			<view class="item radius10 pr flex" v-for="(item,index) in productData" :key="index" >
				<view class="pic" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})"><image src="../../static/images/home-img.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow2" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">闻达香 有机温水稻花香小米+有机绿小米400g</view>
					<view class="B-price">
						<view class="flex">
							<view class="price">79</view>
							<view class="priceTit">销售价</view>
							<view class="price2">￥69</view>
							<view class="priceTit">成本价</view>
						</view>
					</view>
					<view class="addCar"><image src="../../static/images/home-icon9.png" mode=""></image></view>
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
				is_show: false,
				wx_info:{},
				is_show:false,
				curr:0,
				navList:['全部','服装','饰品','箱包','家具','数码','生活','母婴'],
				productData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index
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
	@import "../../assets/style/productList.css";
	@import "../../assets/style/orderNav.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	.orderNav{white-space: nowrap;position: fixed;left: 0;top: 80rpx;right: 0;z-index: 2;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{display: inline-block;margin: 0 30rpx;width: auto;}
</style>
