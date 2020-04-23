<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="table center whiteBj fs13">
				<view class="th flex pubColor">
					<view class="td">下级</view>
					<view class="td">订单总数</view>
					<view class="td">时间</view>
				</view>
				<view class="tr flex" v-for="(item,index) in tableData" :key="index">
					<view class="td">上岛咖啡</view>
					<view class="td">18</view>
					<view class="td">2020.04.23</view>
				</view>
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
				tableData:5
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	page{padding-bottom: 40rpx;background-color: #F5F5F5;}
	
	.table{line-height: 30rpx;}
	.table .th{background-color: #e9f3ff;padding: 25rpx 0;}
	.table .th .td{border-right: 1px solid #38A0F9;padding: 0rpx 10rpx;}
	.table .td{flex: 1;padding: 25rpx 10rpx;box-sizing: border-box;}
	.table .tr .td{border-right: 1px solid #eee;border-bottom: 1px solid #eee;}
	.table .td:last-child{border-right: 0;}
</style>
