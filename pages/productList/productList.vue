<template>
	<view>
		
		<view class="">
			<scroll-view scroll-x class="orderNav whiteBj color6 boxShaow" >
				<view @click="change(-1)" class="tt" :class="!searchItem.category_id?'on':''">全部</view>
				<view @click="change(item.id)" class="tt" :class="searchItem.category_id==item.id?'on':''" v-for="(item,index) in labelData" :key="index">{{item.title}}</view>
			</scroll-view>
		</view>
		<view class="orderNavH"></view>
		<view class="mglr4 productList mgt10">
			<view class="item radius10 pr flex" v-for="(item,index) in mainData" :key="index" >
				<view class="pic" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor">
					<view class="tit avoidOverflow2" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
						{{item.title}}
					</view>
					<view class="B-price">
						<view class="flex">
							<view class="priceTit">销售价</view>
							<view class="price">{{item.o_price}}</view>
							<view class="priceTit" v-if="item.behavior>0">成本价</view>
							<view class="price2" v-if="item.behavior>0">￥{{item.behavior==1?item.member_price:item.price}}</view>
							
						</view>
					</view>
					<view class="addCar" @click="addCar(index)"><image src="../../static/images/home-icon9.png" mode=""></image></view>
				</view>
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				labelData:[],
				mainData:[],
				searchItem:{
					thirdapp_id:2
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.searchItem.category_id = options.id
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
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
			
			addCar(index){
				const self = this;
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
			},
			
			change(id){
				const self = this;
				if(id==-1){
					delete self.searchItem.category_id
					self.getMainData(true)
				}else{
					if(self.searchItem.category_id!=id){
						self.searchItem.category_id = id;
						self.getMainData(true)
					}
				}
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
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				var now = new Date().getTime();
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					} else {
					
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/productList.css";
	@import "../../assets/style/orderNav.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}	
	.orderNav{white-space: nowrap;position: fixed;left: 0;top: 0;right: 0;z-index: 2;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{display: inline-block;margin: 0 30rpx;width: auto;}
</style>
