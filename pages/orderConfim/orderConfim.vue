<template>
	<view>
		<view class="mgt15">
			
			<view class="whiteBj boxShaow oh mgb15">
				<view style="width: 100%;height: 8rpx;"><image src="../../static/images/the-orderl-icon1.png" mode=""></image></view>
				<view class="pdlr4">
					
					<view class="flexRowBetween pdtb15" v-if="addressData.name"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="fs14 color6">
							<view class="flex mgt5">
								<view class="mgr15 color2">{{addressData.name}}</view>
								<view>{{addressData.phone}}</view>
							</view>
							<view class="mgt5">{{addressData.city+addressData.detail}}</view>
						</view>
						<view class="flexEnd" style="width: 20%;">
							<image class="arrowR" src="../../static/images/about-icon-1.png" mode=""></image>
						</view>
					</view>
					
					<view class="flexCenter pdtb25" v-else @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="fs14 color6 flex">
							<view style="width: 40rpx;height:40rpx;margin-right: 20rpx;"><image src="../../static/images/the-orderl-icon.png" mode=""></image></view>
							<view class="fs15">添加地址</view>
						</view>
					</view>
				</view>
			</view>
			<view class="proRow mgt15">
				<view class="item flexRowBetween mgb15" v-for="(item,index) in mainData" :key="index">
					<view class="pic"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2">{{item.product&&item.product.title?item.product.title:''}}</view>
						<view class="flexRowBetween B-price">
							<view class="flex">
								<view class="price">{{item.product&&item.product.o_price?item.product.o_price:''}}</view>
								<view class="priceTit">销售价</view>
								<view class="price2" v-if="userInfoData.behavior>0">
								￥{{userInfoData.behavior==1?item.product.member_price:item.product.price}}
								</view>
								<view class="priceTit">成本价</view>
							</view>
							
							<view class="flexEnd">
								<view class="numBox flex">
									<view class="btn" @click="counter(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view class="btn pubBj white add" @click="counter(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14 ftw">{{totalPrice}}</view></view>
			<button class="payBtn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">立即支付</button>
		</view>
		
		<!-- 订单预约成功弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="payOkShow whiteBj radius10" v-show="is_payOkShow">
			<view class="center color6">订单预定成功！</view>
			<view class="flexCenter pdt25 pdb15">
				<view class="payOkIcon">
					<image src="../../static/images/the-orderl-icon2.png" mode=""></image>
				</view>
			</view>
			<view class="closeBtn" @click="Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})">×</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_payOkShow:false,
				pay:{},
				totalPrice:0,
				mainData:[],
				addressData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.$Utils.loadAll(['getUserData','getDistriData'], self);
			
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getAddressData()
			};
		},
		
		methods: {
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			getUserData() {
				const self = this;
				const postData = {
					searchItem:{thirdapp_id:2}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.countTotalPrice();
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			getDistriData() {
				const self = this;
				const postData = {
					searchItem:{thirdapp_id:2,child_no:uni.getStorageSync('user_info').user_no,type:2}
				};
				postData.tokenFuncName = 'getProjectToken'
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.distriData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getDistriData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			payOkShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_payOkShow = !self.is_payOkShow;
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.member_price = 0;
				console.log('self.userInfoData',self.userInfoData)
				if(self.userInfoData.behavior==1){
					for (var i = 0; i < self.mainData.length; i++) {
						self.totalPrice += self.mainData[i].product.member_price*self.mainData[i].count
						self.member_price += self.mainData[i].product.member_price*self.mainData[i].count
					}
				}else if(self.userInfoData.behavior==2){
					for (var i = 0; i < self.mainData.length; i++) {
						self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
					}
				};
				self.member_price = parseFloat(self.member_price).toFixed(2)
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择收货地址','none')
					return
				}
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({product_id:self.mainData[i].product_id,count:self.mainData[i].count,type:self.mainData[i].type})
				}
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.snap_address = self.addressData;
				console.log('addOrder',self.userInfoData);
				
				if(self.userInfoData.behavior==1){
					postData.type==2;
					postData.data.is_self = 1;
					postData.data.user_no = self.distriData.perent_no;
					postData.data.relation_user = uni.getStorageSync('user_info').user_no;
				}else if(self.userInfoData.behavior==2){
					postData.type==1;
					postData.data.is_self = 0;
				};
				postData.data.member_price = self.member_price;
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder',postData);
			
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						for (var i = 0; i < orderList.length; i++) {
							self.$Utils.delStorageArray('cartData', orderList[i], 'id');
						}
						if(self.userInfoData.behavior==1){
							self.is_show = !self.is_show;
							self.is_payOkShow = !self.is_payOkShow;
						}else if(self.userInfoData.behavior==2){
							self.goPay()
						}
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/userVIP-order/userVIP-order'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/userVIP-order/userVIP-order'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			counter(index, type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.countTotalPrice();
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.editMsg{width: 45%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
	.seltTake .btn{width: 50%;height: 80rpx;line-height: 80rpx;background: #f8f8f8;}
	.seltTake .btn.on{background: #fff;}
	.seltTake .btn:last-child.on{border-radius: 40rpx 0 0 0;}
	
	.xqbotomBar .payBtn{width: 350rpx;}
	
	.payOkShow{width:80%;padding: 50rpx;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.payOkIcon{width:315rpx;height: 329rpx;margin: 0 auto;}
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 14px;
		border-radius: 0;
	}
	button::after{
		border: none;
	}
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
