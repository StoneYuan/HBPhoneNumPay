<template>
	<view>
		<view v-if="loaded || list.itemIndex < 15" class="uni-indexed-list__title-wrapper">
			<text v-if="list.items && list.items.length > 0" class="uni-indexed-list__title">{{ list.key }}</text>
		</view>
		<view v-if="(loaded || list.itemIndex < 15) && list.items && list.items.length > 0"
			class="uni-indexed-list__list">
			<view v-for="(item, index) in list.items" :key="index" class="uni-indexed-list__item"
				hover-class="uni-indexed-list__item--hover">
				<view class="uni-indexed-list__item-container" @click="onClick(idx, index)">
					<view class="uni-indexed-list__item-border"
						:class="{'uni-indexed-list__item-border--last':index===list.items.length-1}">
						<view class="uni-indexed-list_item_contentView">
							<text class="uni-indexed-list__item-content1">{{ item.name }}</text>
							<text class="uni-indexed-list__item-content2">{{ item.phoneNum }}</text>

						</view>
						<view class="uni-indexed-list_item_icon" v-show="item.checked" style="margin-right: 20rpx;">

							<image class="uni-indexed-list_item_image" src="../../static/ic_launcher.png"></image>
						</view>
						<!-- <view class="uni-indexed-list_item_icon">
							<image class="uni-indexed-list_item_image" src="../../static/ic_launcher.png"></image>
						</view> -->
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'UniIndexedList',
		props: {
			loaded: {
				type: Boolean,
				default: false
			},
			idx: {
				type: Number,
				default: 0
			},
			list: {
				type: Object,
				default () {
					return {}
				}
			},
			showSelect: {
				type: Boolean,
				default: false
			}
		},
		methods: {
			onClick(idx, index) {
				this.$emit("itemClick", {
					idx,
					index
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.uni-indexed-list__list {
		background-color: $uni-bg-color;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		border-top-style: solid;
		border-top-width: 1px;
		border-top-color: $uni-border-color;
	}

	.uni-indexed-list__item {
		font-size: $uni-font-size-lg;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}

	.uni-indexed-list__item-container {
		padding-left: $uni-spacing-row-lg;
		flex: 1;
		position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		/* #ifdef H5 */
		cursor: pointer;
		/* #endif */
	}

	.uni-indexed-list__item-border {
		flex: 1;
		position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
		justify-content: space-between;
		// align-items: center;
		height: 55px;
		padding: $uni-spacing-row-lg;
		padding-left: 10rpx;
		margin-top: -5rpx;
		border-bottom-style: solid;
		border-bottom-width: 1px;
		border-bottom-color: $uni-border-color;

	}

	.uni-indexed-list__item-border--last {
		border-bottom-width: 0px;
	}

	.uni-indexed-list_item_contentView {
		display: flex;
		flex-direction: column;

	}

	.uni-indexed-list__item-content1 {
		flex: 1;
		font-size: 16px;
		margin-top: -10rpx;
	}

	.uni-indexed-list__item-content2 {
		flex: 1;
		font-size: 14px;
	}

	.uni-indexed-list_item_icon {
		display: flex;
		align-items: center;
		justify-content: center;
		margin-right: 30rpx;
	}

	.uni-indexed-list_item_image {
		height: 50rpx;
		width: 50rpx;
	}

	.uni-indexed-list {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
	}

	.uni-indexed-list__title-wrapper {
		/* #ifndef APP-NVUE */
		display: flex;
		width: 100%;
		height: 35rpx;
		/* #endif */
		background-color: #f7f7f7;

	}

	.uni-indexed-list__title {
		padding: 0px 12px;
		line-height: 35rpx;
		font-size: $uni-font-size-sm;

	}
</style>
