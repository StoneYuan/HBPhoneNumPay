<template>
	<view class="content">

		<view class="address-search">
			<uni-search-bar @confirm="search" @input="input"></uni-search-bar>
		</view>
		<view class="address-item">
			<uni-indexed-list :options="list" @click="bindClick" :showSelect="true" />

		</view>

		<!-- <view v-for="(item,index) in dataList" :key="item.num">
			<uni-list-item :title="item.name" note=""
				thumb="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-dc-site/460d46d0-4fcc-11eb-8ff1-d5dcf8779628.png"
				thumb-size="lg"></uni-list-item>
		</view> -->

	</view>
</template>

<script>
	// import airport from '../../common/airport.js'
	// import airport from '@/common/airport.js'


	export default {
		components: {},

		data() {
			return {
				'list': [{
					'letter': 'A',
					'data': [{
							name: '阿克苏机场1',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '阿克苏机场2',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '阿克苏机场3',
							phoneNum: '15675565621',
							isHBUser: false,
						},
					]
				},  {
					'letter': 'B',
					'data': [{
							name: '保山机场1',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场2',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场3',
							phoneNum: '15675565621',
							isHBUser: true,
						},
					]
				},
				{
					'letter': 'C',
					'data': [{
							name: '保山机场1',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场2',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场3',
							phoneNum: '15675565621',
							isHBUser: true,
						},
					]
				},{
					'letter': 'D',
					'data': [{
							name: '保山机场1',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场2',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '保山机场3',
							phoneNum: '15675565621',
							isHBUser: true,
						},
					]
				}, {
					'letter': 'Z',
					'data': [{
							name: '振江1',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '振江2',
							phoneNum: '15675565621',
							isHBUser: true,
						},
						{
							name: '振江3',
							phoneNum: '15675565621',
							isHBUser: true,
						},
					]
				}]

			}
		},
		onShow() {
			uni.setNavigationBarTitle({
				title: '通讯录'
			});
		},

		methods: {
			bindClick(e) {
				console.log('点击item，返回数据' + JSON.stringify(e.item))
				
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
	}

	.address-search {}

	.address-item {
		/* margin-top: auto; */
	}

	.topTitle {
		display: flex;
		justify-content: center;
	}
</style>
