<template>
	<view>

		<view class="mglr4 pdt15">
			<view class="table center whiteBj fs13">
				<view class="th flex pubColor">
					<view class="td">我的服务商</view>
					<view class="td">订单总数</view>
					<view class="td">时间</view>
				</view>
				<view class="tr flex" v-for="(item,index) in mainData" :key="index">
					<view class="td">{{item.nickname}}</view>
					<view class="td">{{item.order}}</view>
					<view class="td">{{Utils.timeto(item.up_time*1000,'ymd')}}</view>
				</view>
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils:this.$Utils,
				showView: false,
				score: '',
				wx_info: {},
				is_show: false,
				tableData: 5,
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		
		methods: {

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
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getAchievement(postData, callback);
			},

		}
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	page {
		padding-bottom: 40rpx;
		background-color: #F5F5F5;
	}

	.table {
		line-height: 30rpx;
	}

	.table .th {
		background-color: #e9f3ff;
		padding: 25rpx 0;
	}

	.table .th .td {
		border-right: 1px solid #38A0F9;
		padding: 0rpx 10rpx;
	}

	.table .td {
		flex: 1;
		padding: 25rpx 10rpx;
		box-sizing: border-box;
	}

	
	.table .tr{height: 40px;}
	.table .tr .td{border-right: 1px solid #eee;border-bottom: 1px solid #eee;height: 100%;}

	.table .td:last-child {
		border-right: 0;
	}
</style>
