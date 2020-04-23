<template>
	<view>
		<view class="mgt15">
			
			<view class="whiteBj boxShaow oh mgb15">
				<view style="width: 100%;height: 8rpx;"><image src="../../static/images/the-orderl-icon1.png" mode=""></image></view>
				<view class="pdlr4">
					<view class="flexCenter pdtb25" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="fs14 color6 flex">
							<view style="width: 40rpx;height:40rpx;margin-right: 20rpx;"><image src="../../static/images/the-orderl-icon.png" mode=""></image></view>
							<view class="fs15">添加地址</view>
						</view>
					</view>
					<view class="flexRowBetween pdtb15"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="fs14 color6">
							<view class="flex mgt5">
								<view class="mgr15 color2">张思德</view>
								<view>15689562352</view>
							</view>
							<view class="mgt5">陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="flexEnd" style="width: 20%;">
							<image class="arrowR" src="../../static/images/about-icon-1.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="proRow mgt15">
				<view class="item flexRowBetween mgb15" >
					<view class="pic"><image src="../../static/images/shopping-img.png" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2">闻达香 有机温水稻花香小米+有机绿小米400g</view>
						<view class="flexRowBetween B-price">
							<view class="flex">
								<view class="price">79</view>
								<view class="priceTit">销售价</view>
								<view class="price2">￥69</view>
								<view class="priceTit">成本价</view>
							</view>
							
							<view class="flexEnd">
								<view class="numBox flex">
									<view class="btn" @click="counter('-')">-</view>
									<view class="num">{{count}}</view>
									<view class="btn pubBj white add" @click="counter('+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14 ftw">79</view></view>
			<view class="payBtn"  @click="payOkShow">立即支付</view>
		</view>
		
		<!-- 订单预约成功弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="payOkShow whiteBj radius10" v-show="is_payOkShow">
			<view class="center color6">订单预定成功！</view>
			<view class="flexCenter pdt25 pdb15">
				<view class="payOkIcon">
					<image src="../../static/images/the-orderl-icon2.png" mode=""></image>
				</view>
			</view>
			<view class="closeBtn" @click="payOkShow">×</view>
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
				count:1,
				is_show:false,
				is_payOkShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			payOkShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_payOkShow = !self.is_payOkShow;
			},
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.editMsg{width: 45%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
	.seltTake .btn{width: 50%;height: 80rpx;line-height: 80rpx;background: #f8f8f8;}
	.seltTake .btn.on{background: #fff;}
	.seltTake .btn:last-child.on{border-radius: 40rpx 0 0 0;}
	
	.xqbotomBar .payBtn{width: 350rpx;}
	
	.payOkShow{width:80%;padding: 50rpx;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.payOkIcon{width:315rpx;height: 329rpx;margin: 0 auto;}
	
</style>
