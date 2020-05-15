<template>
	<view>
		<view class="flexCenter">
			<view class="topIcon"><image src="../../static/images/icon.png" mode=""></image></view>
		</view>
		<view class="fs13 myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr"><input type="text" v-model="submitData.name" placeholder="填写姓名" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">手机号</view>
				<view class="rr"><input type="text" v-model="submitData.phone" placeholder="填写手机号" maxlength="11" placeholder-class="placeholder"/></view>
			</view>
			<!-- <view class="item flexRowBetween">
				<view class="ll">邀请码</view>
				<view class="rr">{{submitData.relation_user}}</view>
			</view> -->
			
			<view class="item flexRowBetween">
				<view class="ll">邀请码</view>
				<view class="rr"><input type="text" v-model="submitData.relation_user" placeholder="填写邀请码"  placeholder-class="placeholder"/></view>
			</view>
		</view>
		
		<view class="mglr4 money center">需要支付366元</view>
		<view class="submitbtn">
			<button class="btn" type="default" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
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
				submitData:{
					relation_user:'',
					name:'',
					phone:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(options)
			if(options.scene){
				self.submitData.relation_user = decodeURIComponent(options.scene)
			};
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				if(!pass){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','none')
					return
				};
				const callback = (user, res) => {
					self.addOrder()
				};
				self.$Utils.getAuthSetting(callback);
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
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price)
				const postData = {};	
				postData.wxPay = {
					price:0.01
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							name:self.submitData.name,
							phone:self.submitData.phone
						},
					},
					{
						tableName: 'User',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							behavior:1
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										self.Router.redirectTo({route:{path:'/pages/index/index'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/index/index'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				const postData = {};	
				postData.tokenFuncName = 'getProjectToken',
				postData.data = {
					level:10,
					price:0.01,
					relation_user:self.submitData.relation_user,
				};
				postData.refreshToken = true;
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					}else{
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},

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
