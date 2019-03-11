<template>
	<view>
		<view class="stage" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd" 
		:style="{transform: 'scale('+scale+')'}">
			{{leftDistance}}
		</view>
		<view class="drawer" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd" 
		:class="{transition:status}"
		 :style="{transform: 'translateX('+leftDistance+'px)'}">

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//初始点
				initial: 0,
				//终点
				terminus: 0,
				//是否添加过度属性
				status: false,
				//屏幕宽度
				windowWidth: 0,
				//运动轨迹
				motion: 0,
				//距离左边距的距离
				leftDistance:0,
				type:0,
			};
		},
		computed: {
			scale(){
				if(this.leftDistance/this.windowWidth<0.95){
					return 0.95
				}else{
					return this.leftDistance/this.windowWidth
				}
			}

		},
		onLoad() {
			let that = this;
			uni.getSystemInfo({
				success: function(res) {
					that.windowWidth = res.windowWidth;
					that.leftDistance=res.windowWidth;
				}
			});
		},
		methods: {
			//当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发。 leftDistance==0 ==windowWidth
			updateStart: function(e) {
				this.status = false
				this.motion = 0;
				this.terminus = 0;
				this.initial = e.touches[0].pageX;
			},

			//当手指在屏幕上滑动的时候连续地触发。在这个事件发生期间，调用preventDefault()事件可以阻止滚动。
			updateMove: function(e) {
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
				
				if(this.type==1){
					this.leftDistance=this.terminus-this.initial>0?this.terminus-this.initial:0;
				}else{
					console.info(this.terminus)
					this.leftDistance=this.windowWidth+this.terminus-this.initial
				}
				
			},
			//当手指从屏幕上离开的时候触发。
			updateEnd: function(e) {
				if(this.motion==0){
					return false;
				}
				if(this.windowWidth-this.leftDistance>0){
					this.status=true
				}
				if(this.motion==2){
					this.leftDistance=this.windowWidth;
					this.type=2
					
				}else{
					this.leftDistance=0
					this.type=1
				}
				this.initial=0
			}
		},
	}
</script>

<style lang="less">
	.stage {
		width: 100vw;
		min-height: 100vh;
		background: #FF0000;
		transition: all 0.7s linear;
	}

	.drawer {
		width: 100vw;
		min-height: 100vh;
		background: #5F9EA0;
		position: fixed;
		left: 0;
		top: 0;

		&.transition {
			transition: all 0.3s linear;
		}
	}
</style>
