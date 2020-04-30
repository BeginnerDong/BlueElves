<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll">
					<view>{{item.trade_info}}</view>
					<view class="time">{{item.create_time}}</view>
				</view>
				<view class="rr red">{{item.count}}</view>
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
				is_show: false,
				wx_info:{},
				integralData:4,
				userInfoData:{},
				searchItem:{
					thirdapp_id:2,
					type:2,
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}else{
						
					};
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}	
	.myRowBetween .item{padding: 30rpx 4%;}
	.myRowBetween .ll .time{color: #666;font-size: 26rpx; margin-top: 10rpx;}
	.myRowBetween .item .rr{font-size: 32rpx;}
</style>
