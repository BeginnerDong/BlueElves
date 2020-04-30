<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr f5bj" style="width: 85%; border-radius: 40rpx;">
				<button class="seachBtn" type="button">
					<image src="../../static/images/home-icon.png" mode=""></image>
				</button>
				<view class="input">
					<input type="text" v-model="title"  placeholder="搜索您想要的商品" placeholder-class="placeholder" />
				</view>
				<view class="delt flexCenter" v-if="title!=''" @click="deleteKey"><text>×</text></view>
			</view>
			<view class="fs15 pubColor" @click="search">搜索</view>
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
							<view class="price">{{item.o_price}}</view>
							<view class="priceTit">销售价</view>
							<view class="price2" v-if="item.behavior>0">￥{{item.behavior==1?item.member_price:item.price}}</view>
							<view class="priceTit" v-if="item.behavior>0">成本价</view>
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
				
				title:'',
				mainData:[],
				searchItem:{
					thirdapp_id:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			// self.$Utils.loadAll(['getMainData'], self);
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
			
			deleteKey(){
				const self = this;
				self.title = ''
			},
			
			search(){
				const self = this;
				if(self.title!=''){
					self.searchItem.title = ['like',['%'+self.title+'%']];
				}else{
					delete self.searchItem.title
				}
				self.getMainData()
			},
			
			getMainData(isNew) {
				const self = this;
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/productList.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
</style>

