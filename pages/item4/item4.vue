<style lang="less" scoped>
	.anim1{
		animation: scrollChild 10s linear infinite alternate;
	}
	@keyframes scrollChild {
	0% {
	transform: translateY(0);
	}
	to {
	transform: translateY(-100%);
	}
	}

.anim2{
		animation: scrollChild2 20s linear infinite alternate;
	}
	@keyframes scrollChild2 {
	0% {
	transform: translateX(0);
	}
	to {
	transform: translateX(-100%);
	}
	}
	.lr{
 		width: 100vw;
		height: 100upx;
		border: solid red 1px;
	}
	.lr-item{
		//超出部分隐藏
		overflow: hidden;
		//不准换行
		white-space: nowrap;
		//浮动
		float: left;
		}
	.lr-item view{
		width: 100upx;
		//块状元素转行内元素
		display: inline-block;
		height: 100upx;
		line-height: 100upx;
	}
		
		
</style>
<template>
	<view>
		<view>
			<view  style="margin-top: 100upx;margin-bottom: 50upx;">上下公告</view>
			<view style="border: #FF0000 solid 1upx;overflow: hidden;width: 99.5vw;height: 200upx;" >
				<view :class="[list?'anim1':'']">
					<view  v-for="(item,index) in list" :key='index'>{{index}}</view>
				</view>
			</view>
		</view>
		<view >
			<view style="margin-top: 100upx;margin-bottom: 50upx;">左右公告</view>
			<view class="lr" >
				<view class="lr-item anim2" >
					<view  v-for="(item,index) in list" :key='index'>{{index}}</view>
				</view>
			</view>	
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18]
			};
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});}
	}
</script>


