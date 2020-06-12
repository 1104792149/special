<template>
	<view>
		<view class="contents">
			<h1 class="">我是title</h1>
			<view class="nav" style="display: flex;flex-direction: row;align-items: center;justify-content: space-around;width: 100vw;background-color: #007AFF;">
				<view>单数</view>
				<view>双数</view>
			</view>
			<view v-for="(itme, index) in 60">{{ index }}</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {};
	},
	methods: {},
	/**
	 * 生命周期函数--监听页面加载
	 */
	onLoad(e) {}
};
</script>

<style>
content {
	width: 100%;
	height: 500px;
}
.nav {
	display: table;
	width: 100%;
	position: sticky;
	top: 80upx;
	padding: 10upx 0;
}
</style>
