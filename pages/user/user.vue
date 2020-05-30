<template>
	<view>
		
		<view class="userHead whiteBj pdlr4 pdb15">
			<view class="infor pdtb15">
				<view class="flex">
					<view class="photo mgr10" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
					<view style="width: 78%;">
						<view class="flex">
							<view class="fs15 ftw pdb5" ><open-data type="userNickName"></open-data></view>
							<view class="mgl10 mgb5 titVIP fs12 white">类型：会员</view>
						</view>
						<view class="flex">
							<view class="IDicon"><image src="../../static/images/about-icon0.png" mode=""></image></view>
							<view>ID:{{userInfoData.user_no?userInfoData.user_no:'*******'}}</view>
						</view>
					</view>
				</view>
			</view>
			<view class="vipEquity flexRowBetween pdlr4 pr" @click="Router.navigateTo({route:{path:'/pages/user_vipEquity/user_vipEquity?type=member'}})">
				<view class="bj"><image src="../../static/images/about-img1.png" mode=""></image></view>
				<view class="infor flexRowBetween">
					<view class="flex">
						<image style="width: 53rpx;height: 45rpx;" src="../../static/images/about-icon1.png" mode=""></image>
						<view class="mgl15 fs18 ftw white">会员权益</view>
					</view>
					<view class="btn">去查看</view>
				</view>
			</view>
		</view>
		
		<!-- 我的订单 -->
		<view class="pdlr4 mgt15">
			<view class="userBox2 boxShaow radius10 whiteBj pdlr4">
				<view class="flexRowBetween pdt15 pdb10 borderB1">
					<view class="ftw fs15">我的订单</view>
					<view class="more flexEnd fs12 color9" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">全部订单<image class="arrowR" src="../../static/images/about-icon-1.png" mode=""></image></view>
				</view>
				<view class="menu flexRowBetween fs12 pdt20 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder?num=2'}})">
						<view class="icon">
							<image src="../../static/images/about-icon2.png"></image>
						</view>
						<view>待处理</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder?num=3'}})">
						<view class="icon">
							<image src="../../static/images/about-icon3.png"></image>
						</view>
						<view>待发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder?num=4'}})">
						<view class="icon">
							<image src="../../static/images/about-icon4.png"></image>
						</view>
						<view>已发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder?num=5'}})">
						<view class="icon">
							<image src="../../static/images/about-icon5.png"></image>
						</view>
						<view>已完成</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 管理工具 -->
		<view class="pdlr4 mgt15">
			<view class="userBox2 boxShaow radius10 whiteBj pdlr4">
				<view class="pdt15 pdb10 borderB1">
					<view class="ftw fs15">管理工具</view>
				</view>
				<view class="menu flex fs12 pdt20 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myIntegral/user-myIntegral'}})">
						<view class="icon">
							<image src="../../static/images/about-icon6.png"></image>
						</view>
						<view>我的余额</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myTeam/user-myTeam'}})">
						<view class="icon">
							<image src="../../static/images/about-icon7.png"></image>
						</view>
						<view>我的团队</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myNotice/user-myNotice'}})">
						<view class="icon">
							<image src="../../static/images/about-icon8.png"></image>
						</view>
						<view>我的公告</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="icon">
							<image src="../../static/images/about-icon9.png"></image>
						</view>
						<view>收货地址</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-data/user-data'}})">
						<view class="icon">
							<image src="../../static/images/about-icon10.png"></image>
						</view>
						<view>数据统计</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myAssess/user-myAssess'}})">
						<view class="icon">
							<image src="../../static/images/about-icon11.png"></image>
						</view>
						<view>我的考核</view>
					</view>
				</view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img pr">
					<image src="../../static/images/nabar2.png" />
					<view class="carNum" v-if="cartData.length>0">{{cartCount}}</view>
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" v-if="userInfoData&&userInfoData.behavior<=1"
			 @click="Router.rediressctTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
			<view class="navbar_item" v-if="userInfoData&&userInfoData.behavior==2" 
			@click="Router.redirectTo({route:{path:'/pages/userVIP/userVIP'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">服务商</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userInfoData:{},
				cartCount:0
			}
		},
		
		onLoad() {
			const self = this;
			self.cartData = self.$Utils.getStorageArray('cartData');
			for (var i = 0; i < self.cartData.length; i++) {
				self.cartCount += parseInt( self.cartData[i].count)
			};
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
	.userHead{box-sizing: border-box;position: relative;width: 100%;/* height: 220rpx; */}
	.userHead .photo{width: 100rpx;height: 100rpx;border-radius: 50%;margin-right: 20rpx;border: 1px solid #a5d6ff;;box-sizing: border-box;}
	/* .userHead .infor{position: absolute;top: 0; right: 0; left: 0;} */
	
	.titVIP{padding: 0 14rpx 0 10rpx;line-height: 40rpx;background-image: radial-gradient(to bottom,#f9b135,#ff8010);border-radius: 16rpx 20rpx 20rpx 0;}
	.IDicon{width: 29rpx;height: 25rpx;margin-right: 10rpx;}
	.vipEquity{height: 130rpx;box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.1);}
	.vipEquity .bj{width: 100%;box-sizing: border-box;position: absolute;top: 0;right: 0;bottom: 0;left: 0;}
	.vipEquity .infor{position: relative;z-index: 1;width: 100%;}
	.vipEquity .btn{width: 130rpx;line-height: 50rpx;text-align: center;color: #d2ac74;background-color: #fff;border-radius: 30rpx;}
	/* .myRowBetween .ll .icon{width: 30rpx;height: 32rpx;margin-right: 20rpx;} */
	
	.userBox2{width: 100%;box-sizing: border-box;}
	.userBox2 .menu{flex-wrap: wrap;}
	.userBox2 .item{width: 25%;margin-bottom: 40rpx;}
	.userBox2 .item .icon{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 10rpx;position: relative;}
	
</style>
