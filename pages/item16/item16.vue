<template>
	<view>
		<view v-for="(item,index) in arr" :key='index' class="list">
			<view class="list-border">
				<img class='listImg' :src="item.imgUrl" alt="吖.网络出错了" />
			</view>
			<text class="list-title">标题{{index}}</text>
		</view>
		<view class="list-bottom">
			到底了~
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				arr: [],
				numIndex: 0, //存储图片加载到的位置，避免每次都从第一张图片开始遍历
				imgHtml: [],
				temporary: '../../static/bag.png',
				maxIndex: 3,
				currentIndex: 0,
				ioObj: null
			};
		},
		methods: {
			getData() {
				var that = this;
				for (var i = 0; i < 30; i++) {
					var obj = {
						title: '',
						imgUrl: 'http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg'
					}
					if (i % 2 == 0) {
						obj.imgUrl = 'http://ww4.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg'
					}
					this.arr.push(obj);
				};
				// 				this.IntersectionObserver();
				// 				setTimeout(function() {
				// 					that.checkImgs()
				// 				}, 300);
			},
			//初始化观察	className  // 关闭观察器 io.disconnect();
			IntersectionObserver() {
				var that = this;
				this.ioObj = new IntersectionObserver(function(ioes) {
					// console.info(ioes)
					ioes.forEach(function(ioe) {
						const el = ioe.target;
						const intersectionRatio = ioe.intersectionRatio;
						if (intersectionRatio > 0 && intersectionRatio <= 1) {
							//可见的
							setTimeout(function() {
								el.firstChild.hidden = false;
								console.info(el.className)
								if ('list-bottom' == el.className) {
									// 加载更多
									that.getData();
								}
							}, 200)
						} else {
							//不可见
							el.firstChild.hidden = true
						}
						el.onload = el.onerror = () => that.ioObj.unobserve(el); //停止观察
					});
				});
			},
			//观察全部 方案一
			checkImgs: function() {
				var that = this;
				const imgs = document.getElementsByClassName("list-border");
				// console.info(imgs);
				for (var i = this.numIndex; i < this.arrData.length; i++) {
					that.ioObj.observe(imgs[i]);
				};
				var list_bottom = document.getElementsByClassName("list-bottom");
				that.ioObj.observe(list_bottom[0]);
			},
			// 方案2
			checkImgs2: function() {
				var that = this;
				const imgs = document.querySelectorAll('.list-border');
				const list_bottom = document.querySelectorAll('.list-bottom');
				var newArr1 = Array.from(imgs); //Array.from() 方法从一个类似数组或可迭代对象中创建一个新的数组实例
				var newArr2 = Array.from(list_bottom);
				newArr1.push.apply(newArr1, newArr2);
				newArr1.forEach(el => {
					if (el.className == 'list-bottom') {
						that.loadBottom(el);
					}
					that.loadImg(el)
				})
			},
			loadBottom: function(el) {
				if (this.isBottom(el)) {
					// 加载更多
					this.getData()
				}
			},
			isBottom: function(el) {
				const bound = el.getBoundingClientRect();
				//可视区域的高度
				const clientHeight = window.innerHeight;
				return bound.top <= clientHeight + bound.height * 3
			},
			loadImg: function(el) {
				if (this.isInSight(el)) {
					el.firstChild.hidden = false;
				} else {
					el.firstChild.hidden = true;
				}
			},
			isInSight: function(el) {
				const bound = el.getBoundingClientRect();
				//可视区域的高度
				const clientHeight = window.innerHeight;
				//return bound.top <= clientHeight && bound.top > -bound.height; //测试
				return bound.top <= clientHeight * 1.5 && bound.top > -clientHeight * 2; //正式
			},
			// 简单的节流函数
			//fun 要执行的函数
			//delay 延迟
			//time  在time时间内必须执行一次
			throttle: function(fun, delay, time) {
				var timeout,
					startTime = new Date();
				return function() {
					var context = this,
						args = arguments,
						curTime = new Date();
					clearTimeout(timeout);
					// 如果达到了规定的触发时间间隔，触发 handler
					if (curTime - startTime >= time) {
						fun.apply(context, args);
						startTime = curTime;
						// 没达到触发间隔，重新设定定时器
					} else {
						timeout = setTimeout(function() {
							fun.apply(context, args);
						}, delay);
					}
				};
			},
		},
		computed: {
			arrData() {
				var obj = {
					length: 0,
				}
				obj.length = this.arr.length;
				return obj;
			},
			defaultStatus() {
				if (this.arrData.length == 0 && this.currentIndex == 0) {
					return -1; //初始状态
				} else if (this.arrData.length == 0 && this.currentIndex != 0) {
					return 0; //暂无数据
				} else if (this.currentIndex < this.maxIndex) {
					return 1; //有数据且没加载完毕
				} else if (this.currentIndex == this.maxIndex) {
					return 2; //有数据，已加载完毕
				}
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});
			var that = this;
			// 滚动事件监听
			window.addEventListener('scroll', that.throttle(that.checkImgs2, 200, 400));
			this.getData();
		}
	}
</script>

<style>
	.list {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		padding: 10px;
		border: solid 1px #f2f2f2;
		margin-bottom: 10px;
		font-size: 12px;
	}

	.list-title {}

	.list-border {
		width: 100px;
		height: 100px;
	}

	.listImg {
		height: 100%;
		width: 100%;
	}

	.list-bottom {
		width: 100vw;
		color: #666;
		text-align: center;
		padding: 10px 0;
	}
</style>
