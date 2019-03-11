<template>
	<view class="qj">
		<view class="stage" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd">
			{{leftDistance}}
		</view>
		<view class="ct" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd" 
		:class="{transition:status}" :style="{transform: 'translateX('+leftDistance+'px)'}" >
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//初始点
				initial :0,
				//终点
				terminus:0,
				//是否添加过度属性
				status:false,
				//屏幕宽度
				windowWidth:0,
				//运动轨迹
				motion:0,
			};
		},
		computed: {
			//距离左边距的距离
			leftDistance(){
				if(this.status){
					if(this.motion==1){
						return 0;
					}else if(this.motion==2){
						return this.windowWidth;
					}
				}
				if(this.terminus==0&&this.initial==0){
					return this.windowWidth
				}
				if(this.initial>0&&this.terminus>0){
					return 	this.windowWidth+this.terminus-this.initial	
				}
			}

		},
		onLoad() {
			let that=this;
			uni.getSystemInfo({
			success: function (res) {
				that.windowWidth= res.windowWidth;
			}
			});
		},
		methods: {
			//当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发。
			updateStart: function(e) {
					this.status=false
					this.motion=0;
					this.terminus=0	;
					this.initial = e.touches[0].pageX;
			},
			
			//当手指在屏幕上滑动的时候连续地触发。在这个事件发生期间，调用preventDefault()事件可以阻止滚动。
			updateMove: function(e) {
				if(this.leftDistance>this.windowWidth){
					return false;
				}
				if(this.terminus!=0){
					if(e.touches[0].pageX-this.terminus>0){
						this.motion=2
						console.info('向右滑动')
					}else{
						this.motion=1
						console.info('向左滑动')
					}	
				}
				this.terminus = e.touches[0].pageX;
			},
			//当手指从屏幕上离开的时候触发。
			updateEnd: function(e) {
				if(this.windowWidth-this.leftDistance>0){
					this.status=true
				}
				if(this.leftDistance==0){
					return false;
				}
				this.initial=0
				this.terminus=0	
			}
		},
	}
</script>

<style lang="less">
	
.stage{
	width: 100vw;
	min-height: 100vh;
	background: #FF0000;
	
}
.ct{
	width: 100vw;
	min-height: 100vh;
	background: #5F9EA0;
	position: fixed;left: 0;top: 0;
	&.transition {
	    transition: all 0.3s linear;
	}
}

</style>
