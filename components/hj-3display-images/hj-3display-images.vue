<template>
	<view class="con" :style="{'margin-bottom':btm}">
		<view class="stage" @touchstart="updateStart" @touchmove="updateMove" @touchend="updateEnd">
			<view class="box" :style="{transform:'rotateY('+ss+'deg)'}" :class="[vtouch&&touching?'':'slow']">
				<image :src="src" mode="" v-for="(src,index) in images" :key="index" :style="{transform: 'rotateY('+(index*de)+'deg) translateZ('+wi+')'}"
				 @tap="tran(index)" :class="[vertical?'hrect-image':'vrect-image']"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "hj-3display-images",
		props: {
			images: {
				type: Array,
			},
			//长方形或者矩形
			vertical: {
				type: Boolean,
				default: true
			},
			//是否可以左右滑动
			vtouch: {
				type: Boolean,
				default: true
			}
		},
		computed: {
			de: function() {
				return 360 / this.images.length
			},
			wi: function() {
				return uni.upx2px(350) + 'px';
			},
			btm: function() {
				if (this.vertical) return uni.upx2px(240) + 'px';
				else return uni.upx2px(740) + 'px';
			}
		},
		data() {
			return {
				current: 0,
				ss: 0,
				start: null,
				last: null,
				touching: false,
				temp: 0
			}
		},
		methods: {
			show(){
				let that=this;
				setInterval(()=>{
					that.tran(that.current==that.images.length?0:that.current+1);
				},2000)
			},
			tran: function(index) {
				let old = this.current
				this.current = index;
				let c = (index - old) * this.de
				if (Math.abs(c) > 180) {
					if (c > 0) c = Math.abs(c) - 360
					else c = 360 - Math.abs(c)
				}
				this.ss += -c;
				this.$emit('callbackTap1',  this.current)
			},
			//当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发。
			updateStart: function(e) {
				if (this.vtouch) {
					this.touching = true;
					this.start = this.ss;
					this.last = e.touches[0].pageX;
				}
			},
			//当手指在屏幕上滑动的时候连续地触发。在这个事件发生期间，调用preventDefault()事件可以阻止滚动。
			updateMove: function(e) {
				if (this.vtouch) {
					let np = e.touches[0].pageX;
					let tc = np - this.last;
					this.last = np;
					this.ss += tc;
				}
			},
			//当手指从屏幕上离开的时候触发。
			updateEnd: function(e) {
				if (this.vtouch) {
					this.touching = false;
					let fc = this.ss - this.start;
					let c = Math.round(fc / this.de);
					let index = (this.current - c) % 8;
					this.current = index > 0 ? index : (8 + index)
					this.ss = this.start + c * this.de;
					this.$emit('callbackTap1',  this.current)
				}
			}
		},
		created: function() {
			if (this.images.length < 5 || this.images.length > 10)
				console.warn("图片最好控制在5-9张，可以通过处理images实现")
		}
	}
</script>

<style scoped="">
	.con {
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.stage {
		display: block;
		position: absolute;
		perspective: 800upx;
	}

	.box {
		transform-style: preserve-3d;
		position: relative;
		width: 240upx;
	}

	.vrect-image {
		position: absolute;
		width: 240upx;
		height: 416upx;
		box-shadow: -3upx 5upx 3upx rgba(0, 0, 0, 0.3);
	}

	.hrect-image {
		position: absolute;
		width: 240upx;
		height: 135upx;
		box-shadow: -3upx 5upx 3upx rgba(0, 0, 0, 0.3);
	}

	.slow {
		transition-duration: 1s;
	}
</style>
