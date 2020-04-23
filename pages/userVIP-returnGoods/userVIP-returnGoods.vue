<template>
	<view>
		
		<view class="mglr4">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">待发货</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/shopping-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">墨西哥牛油果8枚单果200g左右</view>
							<view class="B-price flexRowBetween">
								<view class="flex">
									<view class="price">79</view>
									<view class="priceTit">销售价</view>
									<view class="price2">￥69</view>
									<view class="priceTit">成本价</view>
								</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10 mgt15">
				<textarea value="" placeholder="请输入退货原因" placeholder-class="placeholder"/>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<view class="btn">确定</view>
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
				curr:1,
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
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	
	.proRow .item{margin-top: 30rpx;padding-top: 20rpx;}
	.proRow .item .infor{width: 68%;}
	
</style>
