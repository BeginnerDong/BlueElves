<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="(item,index) in integralData" :key="index">
				<view class="ll">
					<view>推广积分</view>
					<view class="time">2019-04-30</view>
				</view>
				<view class="rr red">+5</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				integralData:4
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
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
