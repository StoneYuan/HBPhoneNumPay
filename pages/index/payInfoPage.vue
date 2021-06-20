<template>
	<view>
		<view class="flex align-center justify-center">
			<image :src="pageData.portraitUrl" alt="" class="avtor rounded-circle">
				<text>{{pageData.name}}</text>
				<text>{{pageData.mobileNo}}</text>
				<text class="check-btn">校验</text>
		</view>
		<view style="font-size: 15px;" class="mt-5 ml-3 ml-2">
			金额
		</view>
		<view class="flex flex-row ml-3 mr-5 mt-5" style="border-bottom: 1rpx solid #EEEEEE;">
			<text style="font-size: 50rpx;">¥</text>
			<input style="font-size: 20px;" class="flex-1 ml-3 mr-10" v-model="money" type="text" value="" />
			<text class="check-btn" @click="limitClick()">限额说明</text>
		</view>
		<view class="flex flex-row mt-3 ml-2">
			<text style="font-size: 13px;">到账时间</text>
			<view class="flex flex-column align-start">
				<text style="font-size: 13px;">2小时到账</text>
				<text style="font-size: 13px; color: #c6bec6;">直接转到对方账户，不支持退款</text>
			</view>
			<text class="ml-10" style="font-size: 13px; color:#c99835;">更换</text>
		</view>
		<view class="flex flex-row mt-3 ml-2">
			<text style="font-size: 13px;">备注信息</text>
			<input class="ml-4 justify-center " type="text" style="font-size: 13px; margin-top: 8rpx;" value=""
				placeholder="20字以内(选填)" />
		</view>
		<!-- <button type="default" class="mt-3" @click="goPayClick()">测试数据接口</button> -->

		<button type="default" class="nextButton" @click="goIposClick()">确认转账</button>

	</view>
</template>

<script>
	var hebao = uni.requireNativePlugin("HBUniAppModule")

	export default {

		data() {
			return {
				limiData:[],
				dataList: [],
				pageData: {},
				money:''
			}
		},
		onLoad(options) {
			var data = JSON.parse(options.index); // 字符串转对象
			this.pageData = data;
			console.log(data)
			
			uni.onNativeEventReceive((event, data) => {
				console.log('接收到宿主App消息：' + event + data)
				if (event === 'IPOS_EVENT_H5'){
					console.log('接收到宿主App消息,支付插件特定的事件，做特定页面跳转')
				
				}
				uni.navigateTo({
					url: './paySuccess?index=' + data
				})
			})  
			//
			uni.sendNativeEvent('sendMcaRequest', {
				url: 'mca/pay/v1/payment/limit-amount',
				type: 'get',
				params: {
					type:'1'
				}
			}, ret => {
				
				console.log('接收到宿主App消息：' + ret)
				if (ret.result === 'ok') {
					let data = ret.data;
					let resp = JSON.parse(data);
					let obj = resp.body;
					let arr = [];
					let tips = obj.tips;
					
					for (let i = 0; i < obj.limitAmountList.length; i++) {
						let item = obj.limitAmountList[i]
						var limits = {}
						this.$set(limits,'bankName',item.bankName);
						this.$set(limits,'limitString',item.limitString);
						arr.push(limits)

					}
					this.tips = tips
					this.limiData = arr
					
					console.log('接收到宿主App 限额说明：' + tips + arr)
				}
			})
		},

		methods: {

			goPayClick() {

				// 向宿主App发送事件  
				uni.sendNativeEvent('sendMcaRequest', {
					url: 'mca/pay/v1/transfer/records',
					type: 'get',
					params: {}
				}, ret => {
					this.nativeMsg = '宿主App回传的数据：' + ret;
				})



			},
			goIposClick() {
				
				let price = this.money
				
				console.log('输入的价格 ' + price)
				// 如果价格不正确，不往下执行
				
				// 向宿主App发送事件  
				uni.sendNativeEvent('sendMcaRequest', {
					url: 'mca/pay/v1/transfer/cmpay/order',
					type: 'post',
					params: {
						orderType: '0', // 0，非扫一扫，也非体验商户订单；1，扫一扫订单；2，体验商户订单；3，扫一扫和体验商户订单  
						payeeName: '倪昂', // 真实姓名
						paymentAmount: this.money, // 金额
						payeeMobileNo: '15874252636', // 付款人手机号
						arrivalTimeFlag: '0', // 传 ‘0’代表2小时到账 和‘1’代表次日到账
						remark: '演示', // 备注信息
						promptUsers: '1', //
						realNamecheckFlag: true, // 姓名校验是否通过  这样先写死为通过，实际开发在修改
					}
				}, ret => {
					if (ret.result === 'ok') {
						let data = ret.data;
						let resp = JSON.parse(data);
						let obj = resp.body;
						let orderNo = obj.orderToken;

						uni.sendNativeEvent('sendAsyncRequest', {
							name: 'callIpos',
							params: {
								money: '',
								orderId: orderNo
							}
						}, ret => {
							console.log('支付结果:  ' + ret)
							uni.navigateTo({
								url: './paySuccess?index=' + ret
							})
						})
					}
				})
			},
			limitClick() {
				uni.navigateTo({
					url: './limitNotePage?index=' + this.limiData
				})
			}
		
		}


	}
</script>

<style>
	@import "../../common/free.css";

	.avtor {
		width: 80rpx;
		height: 80rpx;
		background-color: red;
	}

	text {
		text-align: center;
		line-height: 50rpx;
		margin-left: 20rpx;
	}

	.check-btn {
		margin-left: 0rpx;
		color: #c99835;
		font-size: 13px;
	}

	.nextButton {
		margin-top: 80rpx;
		margin-left: 40rpx;
		margin-right: 40rpx;
		background: linear-gradient(to right, #fec883, #c99835);
		color: #ffffff;
		/* font-size: 12px; */
		line-height: 35px;
		border-radius: 6px;

	}
</style>
