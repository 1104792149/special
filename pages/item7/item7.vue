<template>
	<view>
		<view style="display: flex;flex-direction: column;align-items: center;margin-top: 200upx;;">
			<view>
				<!-- <image  :animation="animationData" class="img1" src="../../static/5.png"  ></image> -->
			  <image src="../../static/5.png" mode="" class="z_content" v-bind:style="has_transition" v-bind:class="[hasT?'has_transition':'','img1']"></image>
			</view>
		</view>
		<button @click="kasTap">开始</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				animationData:{},
				hasT:false,
				//要转的总角度
				rotateDeg:0,
				//上一次的角度
				sycDeg:0,
				//50金币，x,100金币，50金币，200金币，面膜
				arrDeg:[30,90,150,210,270,330],
				
			};
		},
		 computed: {
		    has_transition() {
		        return 'transform : rotate(' + this.rotateDeg + 'deg)';
		    }
			
		},
		methods:{
			kasTap(){
				var a=this.randomNum();
				this.hasT=true;
				console.info(this.arrDeg[a])
				this.rotateDeg+=360*6+this.arrDeg[a]-this.sycDeg
				this.sycDeg=this.arrDeg[a];
			},
			 randomNum(){ 
				 return Math.floor(Math.random()*(6-0)+0);
			} ,
			
		}
	}
</script>

<style lang="less" scoped>
.img1{
	height: 600upx;
	width: 600upx;
}
.has_transition {
        transition: all 5s;
    }
</style>
