<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view> -->

		<!-- <page-head :title="title"></page-head> -->
		<view class="uni-wrap">

			<view class="uni-flex">
				<label id="label">手机号 </label>

				<view class="uni-input">
					<input class="uni-input" type="number" @input="onInput" v-model="contact" :value="contact" placeholder="支持移动、联通、电信手机号" te />
				</view>

				<button class="uni-btn" @click="getContact()" border: none></button>

			</view>
			<button class="nextButton" @click="doNextClick()">下一步</button>
			<view class="uni-list">
				<view class="uni-zuijin">
					<text>最近</text>
				</view>


				<uni-list class="uni-list">
					<view v-for="(item, index) in dataList" :key="index">
						<uni-list-item :title="item.name.length?item.name:'未知'" :note="item.mobileNo"
							:thumb="item.portraitUrl?item.portraitUrl:'../../static/ico_avatarDefault.png'"
							thumb-size="lg" @click="clickCell(index)" :data-index="index" link></uni-list-item>

					</view>
				</uni-list>

				<!-- <uni-indexed-list :options="dataList" :showSelect="false" @click="bindClick"></uni-indexed-list> -->

			</view>
			<!-- #ifdef MP-WEIXIN || MP-QQ -->
			<!-- #endif -->

		</view>
	</view>
</template>

<script>
	// import New from '/components/New.vue';
	export default {

		data() {
			return {
				dataList: [],
				contact:''
			}
		},

		onLoad() {
			// 向宿主App发送事件
			uni.sendNativeEvent('sendMcaRequest', {
				url: 'mca/pay/v1/transfer/records',
				type: 'get',
				params: {}
			}, ret => {
				if (ret.result === 'ok') {
					let data = ret.data;
					let resp = JSON.parse(data);
					let obj = resp.body;
					let arr = [];
					for (let i = 0; i < obj.transferList.length; i++) {
						let item = obj.transferList[i]
						if (item.type === '0') {
							arr.push(item)
						}
					}
					this.dataList = arr
				}
			})

			// 	var resp = {
			// 		"requestId": "agw106181545580534177705",
			// 		"uri": "/mca/pay/v1/transfer/records",
			// 		"body": {
			// 			"random": "13",
			// 			"timestamp": "20210618154558243",
			// 			"requestTimestamp": "20210618154558220",
			// 			"pageNo": 1,
			// 			"currentNum": 8,
			// 			"nextPageFlg": false,
			// 			"transferList": [{
			// 				"type": "0",
			// 				"mobileNo": "15675133921",
			// 				"bankCode": "",
			// 				"bankCardNo": "",
			// 				"bankName": "",
			// 				"portraitUrl": "https://mca.cmpay.com:28710/fsgroup1/M00/63/23/rBgYc2AFRwKAcVmqAAL__FCkS0w508.jpg",
			// 				"name": "*石"
			// 			}, {
			// 				"type": "0",
			// 				"mobileNo": "15201692919",
			// 				"bankCode": "",
			// 				"bankCardNo": "",
			// 				"bankName": "",
			// 				"portraitUrl": "https://mca.cmpay.com/ccaweb/jsp/mca/appRecomLstMng/images/20200221101203.png",
			// 				"name": "*阳"
			// 			}, {
			// 				"type": "1",
			// 				"mobileNo": "",
			// 				"bankCode": "CCB",
			// 				"bankCardNo": "6217002920145752979",
			// 				"bankName": "中国建设银行",
			// 				"name": "倪昂"
			// 			}, {
			// 				"type": "0",
			// 				"mobileNo": "18474900806",
			// 				"bankCode": "",
			// 				"bankCardNo": "",
			// 				"bankName": "",
			// 				"portraitUrl": "https://mca.cmpay.com/ccaweb/jsp/mca/appRecomLstMng/images/20200221101203.png",
			// 				"name": ""
			// 			}]
			// 		},
			// 		"msgCd": "MCA00000",
			// 		"msgInfo": "transaction is success"
			// 	};

			// let obj = resp.body;
			// let arr = [];
			// for (let i = 0; i < obj.transferList.length; i++) {
			// 	let item = obj.transferList[i]
			// 	if (item.type === '0') {
			// 		arr.push(item)
			// 	}
			// }
			// this.dataList = arr
		},
		methods: {
			getContact() {
				uni.sendNativeEvent('sendAsyncRequest', {
					name: 'getContact',
					params: {}
				}, ret => {
					var phoneNum = ret;
					uni.sendNativeEvent('sendMcaRequest', {
						url: 'mca/base/v1/transfer/real-name',
						type: 'get',
						params: {
							payeeMobileNo: phoneNum
						}
					}, ret => {

						if (ret.result === 'ok') {
							let data = ret.data;
							let resp = JSON.parse(data);
							let obj = resp.body;
							var item = {};
							this.$set(item, 'name', obj.realName);
							this.$set(item, 'portraitUrl', obj.avatarURL);
							this.$set(item, 'mobileNo', obj.phoneNum);
							var navData = JSON.stringify(item);

							uni.navigateTo({
								url: './payInfoPage?index=' + navData
							})
						}
					})

				})
				
			},

			onInput(e) {
				this.contact = e.target.value
				console.log('输入的是:' + this.contact)
			},
			
			doNextClick() {
				
				let contact = this.contact;
				console.log(contact)
				
				var phoneNum = contact;
				uni.sendNativeEvent('sendMcaRequest', {
					url: 'mca/base/v1/transfer/real-name',
					type: 'get',
					params: {
						payeeMobileNo: phoneNum
					}
				}, ret => {
				
					if (ret.result === 'ok') {
						let data = ret.data;
						let resp = JSON.parse(data);
						let obj = resp.body;
						var item = {};
						this.$set(item, 'name', obj.realName);
						this.$set(item, 'portraitUrl', obj.avatarURL);
						this.$set(item, 'mobileNo', obj.phoneNum);
						var navData = JSON.stringify(item);
						
						console.log(navData)
				
						uni.navigateTo({
							url: './payInfoPage?index=' + navData
						})
					}
				})
				
				// uni.navigateTo({
				// 	url: './payInfoPage?index='
				// })
			},
			clickCell(index) {
				let item = this.dataList[index]
				let num = item.mobileNo
				this.contact = num
				var navData = JSON.stringify(item)
				uni.navigateTo({
					url: './payInfoPage?index=' + navData
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #F3F4F6;
	}

	.content {
		display: flex;
		flex-direction: column;
		/* align-items: margin-left; */
		justify-content: center;
	}

	.uni-wrap {}

	.uni-flex {
		flex-direction: row;
		display: flex;
		margin-top: 40rpx;
		margin-left: 40rpx;
	}

	.uni-input {
		margin-left: 10rpx;
		width: 450rpx;

	}

	.uni-btn {
		margin-left: 15rpx;
		margin-right: 30rpx;
		width: 40rpx;
		height: 55rpx;
		background-image: url(../../static/ico_contact.png);
		background-size: 100% 100%;
		background-repeat: no-repeat;


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

	.uni-zuijin {
		margin-top: 30rpx;
		margin-left: 30rpx;
	}

	.uni-list {
		display: flex;
		flex-direction: column;
		margin-top: 20rpx;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}

	#label {
		width: 130;
	}
</style>
