<template>
	<view>
		<view>
			<image  :class="{anim1:zdStatus1}" src="../../static/5.png" mode="aspectFit"></image>
		</view>
		<button @click="zdTap">转动</button>
		<image class="img1" :class="{dql:zdStatus2}" src="../../static/5.png" mode="aspectFit"></image>
		<button @click="dqlTap">上下浮动</button>
		<image class="img1" :class="{anim2:zdStatus3}" src="../../static/5.png" mode="aspectFit"></image>
		<button @click="xtTap">心跳</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				zdStatus1:false,
				zdStatus2:false,
				zdStatus3:false
			};
		},
		methods:{
			zdTap(){
				this.zdStatus1=!this.zdStatus1
			},
			dqlTap(){
				this.zdStatus2=!this.zdStatus2
			},
			xtTap(){
				this.zdStatus3=!this.zdStatus3
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});}
	}
</script>

<style lang="less" scoped>
.anim1{
	animation: scrollChild 3s linear infinite alternate;
}
@keyframes scrollChild {
0% {
	transform:rotate(0) ;
	
}
to {
	transform:rotate(180deg);
}
}
.anim2{
	animation: xt 1s linear infinite alternate;
}
@keyframes xt{
	0%{
		transform: scale(1);
	}
	to{
		transform: scale(0.8);
	}
}
.dql{
   animation: bounce-up 1.4s linear infinite;animation: bounce-up 1.4s linear infinite;
}

 
 @keyframes bounce-up {
  25% {transform: translateY(10px);}
  50%, 100% {transform: translateY(0);}
  75% {transform: translateY(-10px);}
 }
</style>
