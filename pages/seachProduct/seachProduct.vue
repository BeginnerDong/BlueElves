<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr f5bj" style="width: 85%; border-radius: 40rpx;">
				<button class="seachBtn" type="button">
					<image src="../../static/images/home-icon.png" mode=""></image>
				</button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索您想要的商品" placeholder-class="placeholder" />
				</view>
				<view class="delt flexCenter"><text>×</text></view>
			</view>
			<view class="fs15 pubColor">搜索</view>
		</view>
		
		<view class="mglr4 productList mgt15">
			<view class="item radius10 pr flex" v-for="(item,index) in productData" :key="index">
				
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
				showView: false,
				wx_info:{},
				is_show:false,
				productData:[{},{}]
			}
		},
		
		onLoad(options) {
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
</style>

