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
		<button @click="tap(2)">上下 缩放 弹窗效果</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
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
