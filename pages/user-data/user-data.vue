<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15 pdb15">
			<view class="flex">
				<view class="mgr10">类型：</view>
				<view class="setBox">
					<view class="cont">
						<picker @change="bindPickerChange"  :range="labelData" range-key="title">
							<view class="uni-input">{{labelData[index]?labelData[index].title:'请选择'}}</view>
						</picker>
					</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="flex mgt15">
				<view class="mgr10">时间：</view>
				<view class="setBox">
					<view class="cont">
						<picker mode="date" :value='start' @change="changeStartTime">
							<view class="uni-input">{{start==''?'请选择':start}} </view>
						</picker>
					</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
				<view class="setBox" style="margin-left: 80rpx;">
					<view class="cont">
						<picker mode="date" :value='end' @change="changeEndTime">
							<view class="uni-input">{{end==''?'请选择':end}} </view>
						</picker>
					</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<view class="table center mgt15 whiteBj fs13">
			<view class="th flex pubColor">
				<view class="td">数量(个)</view>
				<view class="td">销售价</view>
				<view class="td">会员成本价</view>
			</view>
			<view class="tr flex">
				<view class="td">{{userData.order?userData.order.count:''}}</view>
				<view class="td">{{userData.order?userData.order.o_price:''}}</view>
				<view class="td">{{userData.order?userData.order.member_price:''}}</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				index: 0,
				tableData:3,
				labelData:[],
				searchItem:{
					thirdapp_id:2
				},
				userData:{},
				end:'',
				start:''
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		onPullDownRefresh() {
			const self = this;
			console.log('refresh');
			self.searchItem.category_id = self.labelData[0].id;
			self.end = '';
			self.start = '';
			delete self.searchItem.create_time;
			self.getMainData()	
		},
		
		methods: {
			
			changeStartTime(e){
				const self = this;
				self.start = e.detail.value;
				
				self.startTimestamp = self.$Utils.timeToTimestamp(self.start);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData()
				};
			},
			
			changeEndTime(e){
				const self = this;
				self.end = e.detail.value;
				
				self.endTimestamp = self.$Utils.timeToTimestamp(self.end);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData()
				};
			},
			
			bindPickerChange(e) {
				// 搜索选择分类
				const self = this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index = e.target.value;
				self.searchItem.category_id = self.labelData[self.index].id;
				self.getMainData()
			},
			
			getLabelData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					label:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							title:['in',['商品分类']],
							status:['in',[1]]
						},
						condition:'in'
					}
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.labelData.push.apply(self.labelData, res.info.data);
						self.searchItem.category_id = self.labelData[0].id
						self.getMainData();
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					order: {
						tableName: 'Order',
						searchItem:self.$Utils.cloneForm(self.searchItem),
						middleKey: 'user_no',
						key: 'relation_user',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    self.$Utils.cloneForm(self.searchItem)
						  ],
						  o_price:[
						    'sum',
						    'o_price',
						    self.$Utils.cloneForm(self.searchItem)
						  ],
						  member_price:[
						    'sum',
						    'member_price',
						    self.$Utils.cloneForm(self.searchItem)
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0]
					}
					uni.stopPullDownRefresh();
				};
				self.$apis.userGet(postData, callback);
			},
		}
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	page{padding-bottom: 40rpx;background-color: #F5F5F5;}
	
	.setBox{width: 250rpx;height: 60rpx;line-height: 56rpx;padding: 0 60rpx 0 20rpx;box-sizing: border-box;border: 1px solid #c6c6c6;position: relative;color: #666;}
	.setBox .arrow{width: 20rpx;height: 12rpx;position: absolute;top: 50%;right: 20rpx;transform: translateY(-50%);}
	
	.table{line-height: 30rpx;}
	.table .th{background-color: #e9f3ff;padding: 25rpx 0;}
	.table .th .td{border-right: 1px solid #38A0F9;padding: 0rpx 10rpx;}
	/* .table .th .td:last-child{border-right: 0;} */
	.table .td{flex: 1;padding: 25rpx 10rpx;box-sizing: border-box;}
	.table .tr{height: 40px;}
	.table .tr .td{border-right: 1px solid #eee;border-bottom: 1px solid #eee;height: 100%;}
	.table .td:last-child{border-right: 0;}
</style>
