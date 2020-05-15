<template>
	<view>
		
		<view class="pdlr4">
			<view class="flexEnd pdtb15">
				<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/user-myIntegralDetail/user-myIntegralDetail'}})">
					<view style="width: 30rpx;height: 32rpx;"><image src="../../static/images/integrall-icon1.png" mode=""></image></view>
					<view class="mgl5">查看流水</view>
				</view>
			</view>
			<view class="loginBj pr">
				<image src="../../static/images/integrall-icon.png" mode=""></image>
			</view>
			<view class="color6 mgb10">余额</view>
			<view class="fs24 ftw">￥{{userInfoData.balance?userInfoData.balance:'***'}}</view>
		</view>
		
		<view class="loginCont">
			<view class="item flex mgb10">
				<view class="input">
					<input type="text" v-model="submitData.count" placeholder="请输入提现金额" placeholder-class="placeholder">
				</view>
				
			</view>
			<view class="item flex mgb10">
				
				<view class="input">
					<input type="text" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder">
				</view>
			</view>
			<!-- <view class="item flex mgb10">
				<view class="input">
					<input type="text" v-model="submitData.wechat" placeholder="请输入微信号" placeholder-class="placeholder">
				</view>
			</view> -->
			<view class="item flex mgb10">
				<view class="input">
					<input type="text" v-model="submitData.name " placeholder="请输入姓名" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex mgb10">
				<view class="input">
					<input type="text" v-model="submitData.card_no" placeholder="请输入银行卡号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex mgb10">
				<view class="input">
					<input type="text" v-model="submitData.bank" placeholder="请输入开户行" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item submitbtn mgt15" style="padding: 0;" @click="submit">
				<button class="Wbtn" type="submint">申请提现</button>
			</view>
			
			
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				userInfoData:{},
				submitData:{
					count:'',
					name:'',
					phone:'',
					//wechat:'',
					card_no:'',
					bank:''
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
					self.flowLogAdd()
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全提现信息', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				/* if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				}; */
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:'提现',
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0,
					name:self.submitData.name,
					phone:self.submitData.phone,
					//wechat:self.submitData.wechat,
					card_no:self.submitData.card_no,
					bank:self.submitData.bank,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	
	page{padding-bottom: 60rpx; text-align: center;}	
	.loginBj{width:150rpx;height: 150rpx;margin: 60rpx auto 30rpx auto;}
	
	.loginCont{z-index: 2;background: #fff;padding: 100rpx 75rpx 80rpx 75rpx;box-sizing: border-box;}
	.loginCont .item{border-radius: 10rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #eee;}
	.loginCont .item .icon image{width: 36rpx;height: 36rpx;margin-right: 30rpx;}
	.loginCont .item .input{width: 100%;}
	.loginCont .item .input input{font-size: 26rpx;}
</style>
