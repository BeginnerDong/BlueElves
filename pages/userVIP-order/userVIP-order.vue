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
					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="flexRowBetween mgb10" v-if="item.user&&item.user[0]">
							<view style="font-weight:700">提交人：{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
						</view>
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==1&&item.order_step==0">已发货</view>
							<view class="red" v-if="item.pay_status==0&&item.order_step==0">待处理</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==0&&item.order_step==0">待发货</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==2&&item.order_step==0">已完成</view>
						</view>
						<view class="flexRowBetween" v-for="(c_item,c_index) in item.child" :key="c_index">
							<view class="pic">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">{{c_item.o_price?c_item.o_price:''}}</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥{{c_item.price?c_item.price:''}}</view>
										<view class="priceTit">成本价</view>
									</view>
									<view class="fs13">×{{c_item.count?c_item.count:''}}</view>
								</view>
							</view>
						</view>
						<view class="adviseF5 flexRowBetween f5bj fs13 mgt15" v-if="item.transport_status==1">
							<view class="ftw">快递单号</view>
							<view class="text pdl10 color6">{{item.express_info}}</view>
						</view>
						<view class="mgt15 flexEnd underBtn">
							<view class="Bbtn" @click="orderUpdate(index)" v-if="item.transport_status==1">确认收货</view>
							<view class="Bbtn blue"  v-if="item.pay_status==0" @click="orderUpdate(index,'delete')">删除订单</view>
							<view class="Bbtn blue"  v-if="item.pay_status==0" @click="goPay(index)" >去支付</view>
							<view class="Bbtn" v-if="item.pay_status==1&&item.order_step==0" 
							:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userVIP-returnGoods/userVIP-returnGoods?id='+$event.currentTarget.dataset.id}})">申请售后</view>
						</view>
						
						
						<view class="adviseF5 flexRowBetween f5bj fs13 mgt15" v-if="item.order_step>0">
							<view class="ftw">退货原因</view>
							<view class="text pdl10 color6">{{item.passage1}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				curr:1,
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					level:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.num){
				self.num = options.num
			}
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			if(self.num){
				self.currChange(self.num)
			}else{
				self.getMainData(true)
			}
		},
		
		methods: {
			
			orderUpdate(index,type) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					transport_status:2,
				};
				if(type&&type=='delete'){
					postData.data = {
						status:-1,
					};
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			goPay(index) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				const postData = {
					wxPay:{
						price: parseFloat(self.mainData[index].price)
					}
				}
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.mainData[index].id
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
										self.getMainData(true)
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
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
								self.getMainData(true)
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: '调起支付失败',
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			currChange(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr;
					if(self.curr==1){
						delete self.searchItem.transport_status
						delete self.searchItem.order_step 
						delete self.searchItem.pay_status 
					}else if(self.curr==2){
						delete self.searchItem.transport_status 
						self.searchItem.order_step = 0
						self.searchItem.pay_status = 0
					}else if(self.curr==3){
						self.searchItem.order_step = 0
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
					}else if(self.curr==4){
						self.searchItem.order_step = 0
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 1
					}else if(self.curr==5){
						self.searchItem.order_step = 0
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2
					}else if(self.curr==6){
					    delete self.searchItem.pay_status 
					    delete self.searchItem.transport_status 
						self.searchItem.order_step = ['>',0]
					}
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'relation_user',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 0;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	
	.orderNav scroll-view{white-space: nowrap;}
	.orderNav scroll-view .tt{width: auto;margin: 0 30rpx;display: inline-block;}
	
	.proRow .item{margin-top: 30rpx;padding-top: 20rpx;}
	.proRow .item .infor{width: 68%;}
	
	.adviseF5{align-items: flex-start;padding: 20rpx;}
	.adviseF5 .text{width: 81%;}
</style>
