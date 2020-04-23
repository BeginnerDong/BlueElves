<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15 pdb15">
			<view class="flex">
				<view class="mgr10">类型：</view>
				<view class="setBox">
					<view class="cont">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
					</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="flex mgt15">
				<view class="mgr10">下级：</view>
				<view class="setBox">
					<view class="cont">
						<picker @change="subordinateChange" :value="snbindex" :range="subordinate">
							<view class="uni-input">{{subordinate[snbindex]}}</view>
						</picker>
					</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="flex mgt15">
				<view class="mgr10">时间：</view>
				<view class="setBox">
					<view class="cont">4月20日</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
				<view class="setBox" style="margin-left: 80rpx;">
					<view class="cont">4月30日</view>
					<view class="arrow"><image src="../../static/images/statisticall-icon.png" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<view class="table center mgt15 whiteBj fs13">
			<view class="th flex pubColor">
				<view class="td">下级</view>
				<view class="td">数量(个)</view>
				<view class="td">会员成本价</view>
				<view class="td">服务商成本价</view>
			</view>
			<view class="tr flex" v-for="(item,index) in tableData" :key="index">
				<view class="td">金海洋</view>
				<view class="td">18</view>
				<view class="td">235.56</view>
				<view class="td">152.26</view>
			</view>
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
				index: 0,
				array: ['生活用品', '服饰', '箱包','全部', '灯具'],
				tableData:3,
				snbindex:0,
				subordinate: ['全部', '上岛咖啡', '星巴克','话费说'],
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			bindPickerChange(e) {
				// 搜索选择分类
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
				this.parent_id = this.classLis[this.index].id;
			},
			subordinateChange(e) {
				// 搜索选择分类
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.snbindex = e.target.value
				this.parent_id = this.classLis[this.snbindex].id;
			}
		}
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	page{padding-bottom: 40rpx;background-color: #F5F5F5;}
	
	.setBox{width: 250rpx;height: 60rpx;line-height: 56rpx;padding: 0 60rpx 0 20rpx;box-sizing: border-box;border: 1px solid #c6c6c6;position: relative;color: #666;}
	.setBox .arrow{width: 20rpx;height: 12rpx;position: absolute;top: 50%;right: 20rpx;transform: translateY(-50%);}
	
	.table{line-height: 30rpx;}
	.table .th{background-color: #e9f3ff;padding: 25rpx 0;}
	.table .th .td{border-right: 1px solid #38A0F9;padding: 0rpx 10rpx;}
	/* .table .th .td:last-child{border-right: 0;} */
	.table .td{flex: 1;padding: 25rpx 10rpx;box-sizing: border-box;}
	.table .tr .td{border-right: 1px solid #eee;border-bottom: 1px solid #eee;}
	.table .td:last-child{border-right: 0;}
</style>
