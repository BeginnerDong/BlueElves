<template>
	<view>
		
		<view class="homeHead">
			<view class="seachbox">
				<view class="flex rr whiteBj" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct'}})">
					<button class="seachBtn" type="button"><image src="../../static/images/home-icon.png" mode=""></image></button>
					<view class="input">
						<input type="text" name="" value="" placeholder="搜索活动" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj pdt15 fs13 mglr4 radius10">
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">服装</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">饰品</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">箱包</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">灯具</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">家具</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="tit">数码</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="tit">生活</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="tit">其他</view>
			</view>
		</view>
		
		<view class="mglr4 pdt15">
			<view class="Big-title fs15 ftw">推荐商品</view>
		</view>
		<view class="mglr4 productList mgt15">
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
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img pr">
					<image src="../../static/images/nabar2.png" />
					<view class="carNum">3</view>
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				productData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			// call(){
			// 	uni.makePhoneCall({
			// 		phoneNumber:'15802977845'
			// 	});
			// },
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.homeHead{height:320rpx;background-color: #122433;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;margin-top: -200rpx;position: relative;z-index: 5;height: 390rpx;box-sizing: border-box;padding-bottom: 10rpx;}
	.indHome .item{width: 25%;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 20rpx; }
	.indHome .item image{width:110rpx;height:110rpx; margin: 0 auto 6rpx auto;}
	
	
	
	.Big-title{position: relative;padding-left: 20rpx;z-index: 1;}
	.Big-title::before{content: '';width: 8rpx;height: 16rpx;background-color:#38a0f9;position: absolute;top: 8rpx;left: 0; z-index: -1;}
	
	
</style>
