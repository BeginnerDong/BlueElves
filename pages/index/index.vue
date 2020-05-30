<template>
	<view>
		
		<view class="homeHead">
			<view class="seachbox">
				<view class="flex rr whiteBj" style="width: 100%;" @click="Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct'}})">
					<button class="seachBtn" type="button"><image src="../../static/images/home-icon.png" mode=""></image></button>
					<view class="input">
						<input type="text" disabled="true" placeholder="搜索您想要的商品" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj pdt15 fs13 mglr4 radius10">
			<view class="item" v-for="(item,index) in labelData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
		</view>
		<!-- <button @click="Router.redirectTo({route:{path:'/pages/payEnter/payEnter'}})">购买会员</button> -->
		<view class="mglr4 pdt15">
			<view class="Big-title fs15 ftw">推荐商品</view>
		</view>
		<view class="mglr4 productList mgt15">
			<view class="item radius10 pr flex" v-for="(item,index) in mainData" :key="index">
				<view class="pic"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor">
					<view class="tit avoidOverflow2"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
						{{item.title}}
					</view>
					<view class="B-price">
						<view class="flex">
							<view class="priceTit">销售价</view>
							<view class="price">{{item.o_price}}</view>
							<view class="priceTit" v-if="userInfoData.behavior>0">成本价</view>
							<view class="price2" v-if="userInfoData.behavior>0">￥{{userInfoData.behavior==1?item.member_price:item.price}}</view>
							
						</view>
					</view>
					<view class="addCar" @click="addCar(index)"><image src="../../static/images/home-icon9.png" mode=""></image></view>
				</view>
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="check('/pages/car/car')">
				<view class="nav_img pr">
					<image src="../../static/images/nabar2.png" />
					<view class="carNum" v-if="cartData.length>0">{{cartCount}}</view>
				</view>
				<view class="text">购物车</view>
			</view>
			<button open-type="getUserInfo"  @getuserinfo="check('/pages/user/user')" class="navbar_item" v-if="userInfoData&&userInfoData.behavior<=1">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</button>
			<view class="navbar_item" v-if="userInfoData&&userInfoData.behavior==2" @click="Router.redirectTo({route:{path:'/pages/userVIP/userVIP'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">服务商</view>
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
				labelData:[],
				idArray:[],
				mainData:[],
				cartData:[],
				userInfoData:{},
				cartCount:0
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.cartData = self.$Utils.getStorageArray('cartData');
			for (var i = 0; i < self.cartData.length; i++) {
				self.cartCount += parseInt( self.cartData[i].count)
			};
			self.$Utils.loadAll(['getUserInfoData','getLabelData'], self);
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
			
			check(path){
				const self = this;
				const callback = (user, res) => {
					if(self.userInfoData.behavior>0){
						self.Router.redirectTo({route:{path:path}})
					}else{
						uni.showModal({
							title:'提示',
							content:'成为会员，享受更多服务。',
							showCancel:false,
							success() {
								if(res.confirm){
									self.getUserInfoData()
								}
							}
						})
						
					}
				};
				self.$Utils.getAuthSetting(callback);
				
			},
			
			addCar(index){
				const self = this;
				if(self.userInfoData.behavior>0){
					var array = self.$Utils.getStorageArray('cartData');
					for (var i = 0; i < array.length; i++) {
						if(array[i].id == self.mainData[index].id){
							var target = array[i]
						}
					}
					if(target){
						target.count  = target.count + 1
					}else{
						var target = self.mainData[index];
						target.count = 1;
						target.isSelect = true;
					}
					self.$Utils.showToast('加入成功', 'none');
					self.$Utils.setStorageArray('cartData', target, 'id', 999);
					self.cartData = self.$Utils.getStorageArray('cartData');
					self.cartCount = 0;
					for (var i = 0; i < self.cartData.length; i++) {
						self.cartCount += parseInt( self.cartData[i].count)
					};
				}else{
					uni.showModal({
						title:'提示',
						content:'成为会员，享受更多服务。',
						showCancel:false
					})
				}
			},
			
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
			
			
			
			
			getLabelData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					label:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							title:['in',['商品分类']],
							status:['in',[1]]
						},
						condition:'in'
					}
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id);
						}
					};
					self.getMainData();
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					category_id:['in',self.idArray]
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	
	.homeHead{height:320rpx;background-color: #122433;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;margin-top: -200rpx;position: relative;z-index: 5;height: 390rpx;box-sizing: border-box;padding-bottom: 10rpx;}
	.indHome .item{width: 25%;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 20rpx; }
	.indHome .item image{width:110rpx;height:110rpx; margin: 0 auto 6rpx auto;}
	
	
	
	.Big-title{position: relative;padding-left: 20rpx;z-index: 1;}
	.Big-title::before{content: '';width: 8rpx;height: 16rpx;background-color:#38a0f9;position: absolute;top: 8rpx;left: 0; z-index: -1;}
	
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 14px;
		border-radius: 0;
	}
	button::after{
		border: none;
	}
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
