<template>
	<view>
		<view style="background-color: #e3e5e5; height: 30rpx;">
		</view>
		<view class="flex flex-column mt-3 ml-2" v-for="(item, index) in dataList" :key="index">
			<text class="mt-1 ml-2" style="font-size: 15px;">{{item.bankName}}</text>
			<text class="mt-1 ml-2" style="font-size: 13px; color: #aaa4ab;">{{item.limitString}}</text>
		</view>
		<view style="margin-top: 30rpx; background-color: #eff2f2; height: 690px;">
			<view class="flex flex-column ml-2 mt-4">
				<text class="mt-1 ml-2 mr-3"
					style="font-size: 13px; color: #aaa4ab;">和包会根据银行规则，支付业务场景，交易风险等级进行交易额度限制，具体以实际支付时提示为准</text>

			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: [{
						bankName: '招商银行储蓄卡',
						limitString: '单笔5000.00元,单日10000.00元，单月20000.00元'
					},
					{
						bankName: '余  额',
						limitString: '单笔1000.00元,单日1000.00元，单月20000.00元'
					}
				]
			};
		},
		onLoad(options) {
			var data = JSON.parse(options.index); // 字符串转对象
			console.log(data);
			this.dataList = data;
		},
	}
</script>

<style>
	@import "../../common/free.css";
</style>
