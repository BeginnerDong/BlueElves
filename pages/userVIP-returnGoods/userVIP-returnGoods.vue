<template>
	<view>
		
		<view class="mglr4">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red">待发货</view>
					</view>
					<view class="flexRowBetween"  v-for="(item,index) in mainData.child">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
							<view class="B-price flexRowBetween">
								<view class="flex">
									<view class="price">{{item.o_price?item.o_price:''}}</view>
									<view class="priceTit">销售价</view>
									<view class="price2">￥{{item.price?item.price:''}}</view>
									<view class="priceTit">成本价</view>
								</view>
								<view class="fs13">×{{item.count?item.count:''}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10 mgt15">
				<textarea v-model="passage1" placeholder="请输入退货原因" placeholder-class="placeholder"/>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<view class="btn" @click="$Utils.stopMultiClick(orderUpdate)">确定</view>
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
				passage1:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.passage1==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请描述退款原因', 'none');
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					order_step:1,
					passage1:self.passage1
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{
						id:self.id,
						user_type:0
					}
				};
				postData.tokenFuncName = 'getProjectToken'
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	
	.proRow .item{margin-top: 30rpx;padding-top: 20rpx;}
	.proRow .item .infor{width: 68%;}
	
</style>
