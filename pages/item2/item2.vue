<style scoped lang="less">
.item2-buttom{
	position: fixed;
	top: 0;
	left: 0;
	width: 100vw;
	height:100vh;
	transform: translateY(100vh);
	transition: all 0.5s;
	&.show1{
		transform: translateY(0px);
	}	
}
.item2-top{
	position: fixed;
	top: 0;
	left: 0;
	width: 100vw;
	transform: translateY(0);
	transition: all 0.5s;
	&.show2{
		transform: translateY(300upx);
	}	
}	
</style>
<template>
	<view>
		<view>
			<!-- 下弹窗 -->
			<view class="item2-buttom" :class="{show1:status1}">
				<view style="position: absolute;bottom: 0;background: #5F9EA0;padding: 20upx;width: 100vw;text-align: center;margin-bottom: 100upx;">
					你好我是下弹窗
				</view>
			</view>
			<!-- 下弹窗 -->
			<view class="item2-top" :class="{show2:status2}">
				<view style="position: absolute;bottom: 0;background: #5F9EA0;padding: 20upx;width: 100vw;text-align: center;">
					你好我是上弹窗
				</view>
			</view>
			<button @click="tc1Tap()">下弹窗</button>
			<button style="position: fixed;bottom: 0;width: 100vw;" @click="tc2Tap()">上弹窗</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				status1:false,
				status2:false
			};
		},
		methods:{
			tc1Tap(){
				this.status1=!this.status1
			},
			tc2Tap(){
				this.status2=!this.status2
			}
				
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});}
	}
</script>


