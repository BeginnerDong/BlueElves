<template>
	<view>
		
		<view class="noticeList fs13">
			<view class="item flexRowBetween" v-for="(item,index) in teamData" :key="index">
				<view class="icon"><image src="../../static/images/gonggao-icon.png" mode=""></image></view>
				<view class="infor">
					<view>副驾驶的考虑过公积金付老师的结果看老人家诶工具人VB国际快递女富可敌国九分裤大家好说家居克劳福德经济管理</view>
					<view class="time">2020年4月23日</view>
				</view>
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
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				is_hxEwmShow:false,
				teamData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
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
