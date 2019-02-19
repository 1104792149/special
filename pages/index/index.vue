<style>
	button {
		margin-top: 30upx;
		background-color: red;
		color: #fff;
	}
</style>
<template>
	<view>
		<button @click="tap(1)">抽屉效果</button>
		<button @click="tap(2)">上下 弹窗效果</button>
		<button @click="tap(3)">缩放 弹窗效果</button>
		<button @click="tap(4)">公告 效果</button>
		<button @click="tap(5)">左右滚动 效果</button>
		<button @click="tap(6)">转圈 上下浮动  效果</button>
		<button @click="tap(7)">抽奖转盘</button>
		<button @click="tap(8)">红包雨</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			tap(type) {
				uni.navigateTo({
					url: `../item${type}/item${type}`
				})
			}
		}
	}
</script>
