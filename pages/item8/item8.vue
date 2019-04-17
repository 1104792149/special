<template>
	<view>
		<button @click="showInterTap">{{30000==djs||djs==0?'开始':djs/1000+'秒'}}</button>
		<view v-for="(item,index) in packetList" :key='index'>
			<image v-if="showInter" @click="hbTap(index)" class="red-packet" :class="{anim1:item.status}" :src="item.src" :style="item.style">
			</image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				windowWidth: "", //窗口宽度
				windowHeigh: "", //窗口高度
				packetList: [], //红包队列
				showInter: '', //  循环动画定时器
				djs: 0,
			};
		},
		computed: {

		},
		methods: {
			hbTap(index) {
				uni.showToast({
					title: '中奖红包下标' + index
				})
			},
			styleData() {
				// 生成随机位置（水平位置）
				var left = Math.random() * this.windowWidth - 40;
				// 优化位置，防止红包越界现象，保证每个红包都在屏幕之内
				if (left < 0) {
					left += 40;
				} else if (left > this.windowWidth) {
					left -= 40;
				}
				var speed = Math.random() * 2500 + 3000 //生成随机掉落时间，保证每个掉落时间保持在3秒到5.5秒之间 
				var value = 'left:' + left + 'px;' + 'animation-duration:' + speed + 'ms;';
				return value;
			},
			showInterTap() {
				var that = this;
				this.djs = 0;
				this.showInter = setInterval(() => {
					var a = this.styleData();
					//建立临时红包图片数组
					var srcList = ["../../static/1.png", "../../static/2.png"];
					let obj = {
						style: a,
						status: true,
						src: srcList[Math.ceil(Math.random() * 2) - 1],
					};
					that.packetList.push(obj);
					this.djs += 200;
					if (this.djs == 30000) {
						clearInterval(that.showInter);
					}
				}, 100);
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});
			var that = this;
			// 获取手机屏幕宽高
			uni.getSystemInfo({
				success: function(res) {
					that.windowWidth = res.windowWidth,
						that.windowHeigh = res.windowHeight
				}
			});

		}

	}
</script>

<style lang="less" scoped>
	.red-packet {
		width: 80upx;
		height: 90upx;
		z-index: 100;
		position: fixed;
		top: -160upx;
	}

	.anim1 {
		animation-name: scrollChild;
		animation-timing-function: 'ease';
		animation-delay: 0s;
		animation-iteration-count: 1;
		animation-direction: 'normal'
	}

	@keyframes scrollChild {
		0% {
			transform: translateY(0) rotate(0deg);
		}

		100% {
			transform: translateY(100vh+30px) rotate(360deg);
			opacity: 0;
		}
	}
</style>
