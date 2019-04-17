<style scoped lang="less">
.item1-000{
	width: 100vw;
	height: 100vh;
	background:rgba(34,34,34, 0.4);
	position: fixed;
	top: 0;
	left: 0;
	transform: translateX(100%);
	transition: all 0.7s;
	&.show {
	    transform: translateX(0);
	}
	
}
.item1-fff{
	height: 100%;
	width: 80%;
	margin-left: 20%;
	background: cadetblue;
}
</style>

<template>
	<view>
		<view class="item1-000" :class="{show:status}">
			<view class="item1-fff"></view>
		</view>
		<button @click="ctTap">抽屉效果</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				status:false
			};
		},
		methods:{
			ctTap(){
				this.status=!this.status;
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});}
	}
</script>

