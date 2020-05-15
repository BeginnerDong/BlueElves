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
			<view class="item flexRowBetween" v-if="curr==4&&userInfoData.behavior==2">
				<view class="ll flex">
					<view class="photo"><image src="../../static/images/logo.jpg" mode=""></image></view>
					<view class="infor">
						<view class="fs13">蓝色精灵</view>
						<view class="fs13 mgt5">{{Utils.timeto(userInfoData.up_time*1000,'ymd')}}</view>
					</view>
				</view>
				<view class="rr">类型：平台</view>
			</view>
			<view class="item flexRowBetween" v-else v-for="(item,index) in mainData" :key="index">
				<view class="ll flex">
					<view class="photo"><image :src="item.relationUser&&item.relationUser[0]?item.relationUser[0].headImgUrl:''" mode=""></image></view>
					<view class="infor">
						<view class="fs13">{{item.relationUser&&item.relationUser[0]?item.relationUser[0].nickname:''}}</view>
						<view class="fs13 mgt5">{{item.create_time}}</view>
					</view>
				</view>
				<view class="rr">类型：{{item.relationUser&&item.relationUser[0]&&item.relationUser&&item.relationUser[0].behavior==1?'会员':'服务商'}}</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" style="width: 417rpx;height: 396rpx;margin-top: 200rpx;" v-if="mainData.length==0"><image src="../../static/images/teaml-icon.png" mode=""></image></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				is_hxEwmShow:false,
				teamData:[{},{},{}],
				searchItem:{
					type:2
				},
				mainData:[],
				userInfoData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.parent_no = uni.getStorageSync('user_info').user_no;
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			currChange(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr;
					if(self.curr==1){
						self.searchItem = {
							type:2,
							parent_no:uni.getStorageSync('user_info').user_no
						}
					}else if(self.curr==2){
						self.searchItem = {
							type:1,
							parent_no:uni.getStorageSync('user_info').user_no
						}
					}else if(self.curr==3){
						self.searchItem = {
							
							child_no:uni.getStorageSync('user_info').user_no
						}
					}else if(self.curr==4){
						self.searchItem = {
							type:2,
							child_no:uni.getStorageSync('user_info').user_no
						}
					}
					self.getMainData(true)
				}
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					relationUser:{
						tableName:'User',
						middleKey:self.curr==1||self.curr==2?'child_no':'parent_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						if(self.curr==3){
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].type==2){
									self.mainData.splice(i,1)
								}
							}
						}
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.distriGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/editInfor.css";
	page{/* background: #F5F5F5; */}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 0;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	.myRowBetween .item{padding: 30rpx 4%;}
	
</style>
