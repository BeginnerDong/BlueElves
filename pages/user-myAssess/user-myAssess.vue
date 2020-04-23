<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="(item,index) in asessData" :key="index">
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
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<view class="submitbtn pdtb15">
			<view class="btn" @click="asessShow">考核申请</view>
		</view>
		
		<!-- 考核弹框 -->
		<view class="asessShow white radius10 center" v-show="is_asessShow">
			<view class="closebtn" style="color: #fff;" @click="asessShow">×</view>
			<view>您发展的会员未达到数量，不满足晋升服务的资格。</view>
			<view>您考核申请申请成功，请等待后台审核。</view>
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
				asessData:[{},{},{}],
				is_asessShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			asessShow(){
				const self = this;
				self.is_asessShow = !self.is_asessShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 160rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
	.submitbtn{position: fixed;left: 0;right: 0;bottom: 0;background-color: #fff;border-top: 1px solid #eee;}
	
	.asessShow{width: 80%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);background-color: rgba(0,0,0,0.5);box-sizing: border-box;padding:100rpx 10%;line-height: 48rpx;}
</style>
