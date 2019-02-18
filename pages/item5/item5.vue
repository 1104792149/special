<template>
	<view>
		<view class="container-wrap">
			<view class="container">
				<view  v-for="(item,index) in 20" :key='index'>{{index}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			};
		}
	}
</script>

<style lang="less" scoped>
.container-wrap{
		margin-top: 300upx;
		width: 100vw;
		white-space: nowrap;
		overflow: hidden;
		overflow-x: scroll;
		-webkit-backface-visibility: hidden;
		-webkit-perspective: 1000;
		-webkit-overflow-scrolling: touch;
		}
		.container view{
		display: inline-block;
		height: 50px;
		text-align: center;
		line-height: 50px;
		color: red;
		width: 100upx;
		}
		.container-wrap::-webkit-scrollbar {
        display: none;
		}
</style>
