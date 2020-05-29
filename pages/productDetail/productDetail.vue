<template>
	<view>
		
		<view class="banner-box">
			<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#f8b734" indicator-color="rgba(173,173,173)">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="mglr4 pdtb15 detailTit">
			<view class="fs15 ftw">{{mainData.title}}</view>
			<view class="flex pdt20">
				<view class="priceTit">销售价</view>
				<view class="price">{{mainData.o_price}}</view>
				<view class="priceTit" v-if="userInfoData.behavior>0">成本价</view>
				<view class="price2" v-if="userInfoData.behavior>0">￥{{userInfoData.behavior==1?mainData.member_price:mainData.price}}</view>
				<view class="price2">库存：{{mainData.stock?mainData.stock:0}}</view>
			</view>
			
		</view>
		<view class="f5H10"></view>
		<view class="pdt15" >
			<view class="mglr4 xqInfor">
				<view class="fs15 ftw pdb20">商品详情描述</view>
				<view class="cont fs14 center">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
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
				<view class="ite flexColumn" @click="check('/pages/car/car')">
					<view class="icon pr">
						<image src="../../static/images/detailsl-icon.png" mode=""></image>
						<view class="carNum" v-if="cartData.length>0">{{cartCount}}</view>
					</view>
					<view class="mgt5">购物车</view>
				</view>
			</view>
			<view class="flexEnd">
				<view class="payBtn" @click="addCar" style="background-color: #75b9f3;">加入购物车</view>
				<view class="payBtn" @click="goBuy">立即订购</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				userInfoData:{},
				cartData:[],
				cartCount:0
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.id = options.id;
			self.cartData = self.$Utils.getStorageArray('cartData');
			for (var i = 0; i < self.cartData.length; i++) {
				self.cartCount += parseInt( self.cartData[i].count)
			};
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')&&self.curr==2) {
				self.paginate.currentPage++;
				self.getMessageData()
			};
		},
		
		methods: {
			
			goBuy() {
				const self = this;
				if(self.userInfoData.behavior>0){
					uni.setStorageSync('canClick', false);
					self.orderList.push({
						product_id: self.mainData.id,
						count: 1,
						product: self.mainData
					}, );
					uni.setStorageSync('payPro', self.orderList);
					self.Router.navigateTo({
						route: {
							path: '/pages/orderConfim/orderConfim'
						}
					})
					uni.setStorageSync('canClick', true);
				}else{
					uni.showModal({
						title:'提示',
						content:'成为会员，享受更多服务。',
						showCancel:false
					})
				}
				
			},
			
			check(path){
				const self = this;
				if(self.userInfoData.behavior>0){
					self.Router.redirectTo({route:{path:path}})
				}else{
					uni.showModal({
						title:'提示',
						content:'成为会员，享受更多服务。',
						showCancel:false
					})
				}
			},
			
			addCar(){
				const self = this;
				if(self.userInfoData.behavior>0){
					var array = self.$Utils.getStorageArray('cartData');
					for (var i = 0; i < array.length; i++) {
						if(array[i].id == self.id){
							var target = array[i]
						}
					}
					if(target){
						target.count  = target.count + 1
					}else{
						var target = self.mainData;
						target.count = 1;
						target.isSelect = true;
					}
					self.$Utils.showToast('加入成功', 'none');
					self.$Utils.setStorageArray('cartData', target, 'id', 999);
					self.cartData = self.$Utils.getStorageArray('cartData');
					for (var i = 0; i < self.cartData.length; i++) {
						self.cartCount += parseInt( self.cartData[i].count)
					};
				}else{
					uni.showModal({
						title:'提示',
						content:'成为会员，享受更多服务。',
						showCancel:false
					})
				}
				
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
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
