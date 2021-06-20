<template>
	<view>
		<view style="background-color: #e8ebeb; height: 30rpx;">
		</view>
		<view class="flex flex-column mt-3 align-center">
			<view class="flex justify-center align-center">
				<image src="../../static/ic_launcher.png" mode="" style="width: 40rpx;height: 40rpx;"></image>
				<text class="ml-1">支付成功</text>
			</view>
			<view class="flex align-center mt-5">
				<text class="" style="font-size: 60rpx;">{{pageData.pageData}}</text>
				<text class="ml-1">元</text>
			</view>
		</view>
		<view class="pl-3 mt-3 m-text-info">
			<view class="flex py-1">
				<text class="flex-1">收款方</text>
				<text class="flex-3">158****3829</text>
			</view>
			<view class="flex py-1">
				<text class="flex-1">到账时间</text>
				<text class="flex-3">预计06-18</text>
			</view>
			<view class="flex py-1">
				<text class="flex-1">付款方式</text>
				<text class="flex-3">和包余额</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: [],
				pageData: {}
			}
		},
		onLoad(options) {
			var data = JSON.parse(options.index); // 字符串转对象
			this.pageData = data
		},
	}
</script>

<style>
	@import "../../common/free.css";

	.m-text-info {
		font-size: 30rpx;
		border-top: 1rpx solid #E9E8E5;
	}
</style>
