<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
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
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<view class="submitbtn pdtb15">
			<view class="btn" :style="!can?'background:#999':''" @click="submit">考核申请</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				searchItem:{
					type:1
				},
				asessData:[{},{},{}],
				is_asessShow:false,
				mainData:[],
				can:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.parent_no = uni.getStorageSync('user_info').user_no;
			self.$Utils.loadAll(['getMainData'], self);
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
			
			submit(){
				const self = this;
				if(self.mainData.length<5){
					uni.showModal({
						title:'提示',
						content:'您推荐的会员未达到数量，不满足晋升服务的资格。',
						showCancel:false
					})
				}else{
					const postData = {};
					postData.tokenFuncName = 'getProjectToken';
					postData.searchItem = {
						user_no:uni.getStorageSync('user_info').user_no
					};
					postData.data = {
						check_status:1
					};
					const callback = (res) => {
						if (res.solely_code == 100000) {
							uni.showModal({
								title:'提示',
								content:'您的考核申请申请成功，请等待后台审核。',
								showCancel:false,
								success(res) {
									if(res.confirm){
										uni.navigateBack({
											delta:1
										})
									}
								}
							})
						}else{
							self.$Utils.showToast(res.msg, 'none');
						}
					};
					self.$apis.userUpdate(postData, callback);
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
						middleKey:'child_no',
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
						if(self.mainData.length>=5){
							self.can = true
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 160rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
	.submitbtn{position: fixed;left: 0;right: 0;bottom: 0;background-color: #fff;border-top: 1px solid #eee;}
	
	.asessShow{width: 80%;min-height: 300rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);background-color: rgba(0,0,0,0.5);box-sizing: border-box;padding:100rpx 10%;line-height: 48rpx;}
</style>
