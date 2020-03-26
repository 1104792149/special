<style scoped lang="less">
@-webkit-keyframes fadeIn {
	0% {
		transform: translateX(100%);
	}
	100% {
		transform: translateX(0%);
	}
}
@-webkit-keyframes fadeIn2 {
	0% {
		transform: translateX(0%);
	}
	100% {
		transform: translateX(100%);
	}
}
.item1-000 {
	width: 100vw;
	height: 100vh;
	background: rgba(34, 34, 34, 0.4);
	position: fixed;
	left: 0;
	top: 0;
	transition: all 0.3s linear;
	transform: translateX(100%);
	&.show{
		transform: translateX(0%);
	}
}

.item1-title {
	height: 280upx;
	width: 100%;
	background: red;
	position: absolute;
	top :0;
	left: 0;
	z-index: 2;
}
.item1-value{
	height: 100%;
	width: 100%;
	background: blueviolet;
	position: absolute;
	top :0;
	left: 0;
	z-index: 1;
	text-align: center;
	display: flex;
	flex-direction: column;
	align-items: center;
	padding-top: 500upx;
}
.dh{
	-webkit-animation-name: fadeIn; /*动画名称*/
	-webkit-animation-duration: 0.3s; /*动画持续时间*/
	-webkit-animation-iteration-count: 1; /*动画次数*/
	-webkit-animation-delay: 0s; /*延迟时间*/
	-webkit-animation-timing-function: linear;
}
</style>

<template>
	<view>
		<button @click="status1=!status1">抽屉效果</button>
		<view  class="item1-000" :class="{show:status1}" >
			<view class="item1-title"  @click.stop="status1=!status1"></view>
			<view @click="ctTap2" class="item1-value" :class="[index2>0?'dh':'']" :key="index2" v-for="(itme2, index2) in number2">
				{{index2}}
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			status: false,
			number1: 0,
			status1: false,
			number2: 1
		};
	},
	methods: {
		ctTap2() {
			this.number2++;
			console.log(this.number2);
		},
	},
	onLoad(e) {
		uni.setNavigationBarTitle({
			title: e.title
		});
	}
};
</script>
