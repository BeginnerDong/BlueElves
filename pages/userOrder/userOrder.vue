<template>
	<view>

		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">待处理</view>
				<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">待发货</view>
				<view class="tt" :class="curr==4?'on':''" @click="currChange('4')">已发货</view>
				<view class="tt" :class="curr==5?'on':''" @click="currChange('5')">已完成</view>
			</view>
		</view>
		<view class="topNavH"></view>


		<view>
			<view class="mglr4">
				<view class="proRow">

					<view class="item" v-for="(item,index) in mainData" :key="index">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==1">已发货</view>
							<view class="red" v-if="item.pay_status==0">待处理</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==0">待发货</view>
							<view class="red" v-if="item.pay_status==1&&item.transport_status==2">已完成</view>
						</view>
						<view class=" flexRowBetween" v-for="(c_item,c_index) in item.child" :key="c_index">
							<view class="pic">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="B-price flexRowBetween">
									<view class="flex">
										<view class="price">{{c_item.o_price?c_item.o_price:''}}</view>
										<view class="priceTit">销售价</view>
										<view class="price2">￥{{c_item.member_price?c_item.member_price:''}}</view>
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
						<!-- <view class="mgt15 flexEnd underBtn" @click="" v-if="item.transport_status==1">
							<view class="Bbtn">确认收货</view>
						</view> -->
					</view>

				</view>
			</view>
		</view>

		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0">
			<image src="../../static/images/nodata.png" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				score: '',
				wx_info: {},
				is_show: false,
				curr: 1,
				is_hxEwmShow: false,
				proData: [{}, {}, {}],
				mainData: [],
				searchItem: {
					thirdapp_id: 2,
					level: 1
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
			
			currChange(curr) {
				const self = this;
				if (curr != self.curr) {
					self.curr = curr
					if (self.curr == 1) {
						delete self.searchItem.transport_status
						delete self.searchItem.order_step
						delete self.searchItem.pay_status
					} else if (self.curr == 2) {
						delete self.searchItem.transport_status
						delete self.searchItem.order_step
						self.searchItem.pay_status = 0
					} else if (self.curr == 3) {
						delete self.searchItem.order_step
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
					} else if (self.curr == 4) {
						delete self.searchItem.order_step
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 1
					} else if (self.curr == 5) {
						delete self.searchItem.order_step
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2
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
				postData.searchItem.relation_user = uni.getStorageSync('user_info').user_no;
				postData.searchItem.user_no = ['not in',[uni.getStorageSync('user_info').user_no]];
				postData.searchItem.user_type = 0;
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

	/* @import "../../assets/style/editInfor.css"; */
	page {
		background: #F5F5F5;
	}

	.topNavFix {
		width: 100%;
		height: 80rpx;
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
		box-sizing: border-box;
		z-index: 22;
	}

	.topNavH {
		height: 80rpx;
	}

	.proRow .item {
		margin-top: 30rpx;
		padding-top: 20rpx;
	}

	.proRow .item .infor {
		width: 68%;
	}

	.proList {
		padding-bottom: 20rpx;
	}

	.proList:last-child {
		padding-bottom: 0;
	}

	.hxEwm {
		width: 85rpx;
		height: 85rpx;
	}

	.hxEwmShow {
		width: 70%;
		position: fixed;
		top: 45%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 50;
	}

	.hxEwmShow .ewm {
		width: 400rpx;
		height: 400rpx;
		display: block;
		margin: 0 auto;
	}

	.closeBtn {
		background: rgba(0, 0, 0, 0.5);
		border: 0;
		top: auto;
		bottom: -130rpx;
	}
</style>
