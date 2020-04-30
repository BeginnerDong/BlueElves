<template>
	<view>
		
		<view class="noticeList fs13">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="icon"><image src="../../static/images/gonggao-icon.png" mode=""></image></view>
				<view class="infor">
					<view>{{item.description}}</view>
					<view class="time">{{item.create_time}}</view>
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
			
				mainData:[]
			}
		},
		
		onLoad(options) {
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['公告']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	page{}
	.noticeList .item{padding: 30rpx 4%;align-items: flex-start;border-bottom:20rpx solid #F5F5F5 ;}
	.noticeList .item .icon{width: 70rpx;height: 70rpx;}
	.noticeList .item .infor{width: 87%;}
	.noticeList .item .time{font-size: 24rpx;color: #666;margin-top: 10rpx;}
</style>
