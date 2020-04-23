<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav whiteBj boxShaow color6 mgb15 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">我的成员</view>
				<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">我的推荐</view>
				<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">推荐我的</view>
				<view class="tt" :class="curr==4?'on':''" @click="currChange('4')">我的供货商</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="(item,index) in teamData" :key="index">
				<view class="ll flex">
					<view class="photo"><image src="../../static/images/teaml-img.png" mode=""></image></view>
					<view class="infor">
						<view class="fs13">哆啦A梦</view>
						<view class="fs13 mgt5">2020.04.23</view>
					</view>
				</view>
				<view class="rr">类型：会员</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" style="width: 417rpx;height: 396rpx;margin-top: 200rpx;"><image src="../../static/images/teaml-icon.png" mode=""></image></view>
		
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/editInfor.css";
	page{/* background: #F5F5F5; */}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 80rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
	
</style>
