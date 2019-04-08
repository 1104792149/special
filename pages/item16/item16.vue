<template>
	<view>
		<view v-for="(item,index) in arr" :key='index' class="list">
			<img   class='listImg' :src="temporary" :data-src="item.imgUrl" alt="吖.网络出错了" />
			<text class="list-title">标题{{index}}</text>
		</view>
		<view class="list-bottom" v-if='defaultStatus==2'>
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
				maxIndex:3,
				currentIndex:0
			};
		},
		methods: {
			//window.onscroll = lazyload;
			lazyload: function() { //监听页面滚动事件
			
				var scrollTop =document.documentElement.scrollTop || document.body.scrollTop; //滚动条距离顶部高度
				var windowHeight =document.documentElement.clientHeight;//变量windowHeight是可视区的高度
				var scrollHeight = document.documentElement.scrollHeight;//变量scrollHeight是滚动条的总高度
				
				for (var i = this.numIndex; i < this.arrData.length; i++) {
					//元素是否在可见区域内
					if (this.imgHtml[i].offsetTop < windowHeight + scrollTop) {
						if (this.imgHtml[i].getAttribute("src") == this.temporary) {
							this.imgHtml[i].src = this.imgHtml[i].getAttribute("data-src");
						}
						this.numIndex = i + 1;
					}
				};
				if (scrollTop + windowHeight > scrollHeight-50) {//滚动条到底部的条件
					// 到底部
					this.getData(); 
				}

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
			getData(){
				if(this.currentIndex==this.maxIndex){
					return ''
				}else{
					this.currentIndex++;
				}
				for (var i = 0; i < 10; i++) {
				var obj = {
					title: '',
					imgUrl: 'http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg'
				}
				if (i % 2 == 0) {
					obj.imgUrl = 'http://ww4.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg'
				}
				this.arr.push(obj);
			}
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
			defaultStatus(){
				if(this.arrData.length==0&&this.currentIndex==0){
					return -1;//初始状态
				}else if(this.arrData.length==0&&this.currentIndex!=0){
					return 0;//暂无数据
				}else if(this.currentIndex<this.maxIndex){
					return 1;//有数据且没加载完毕
				}else if(this.currentIndex==this.maxIndex){
					return 2;//有数据，已加载完毕
				}
			}
		},
		mounted: function() {
			var that = this;
			this.getData();

			setTimeout(function() {
				that.imgHtml = document.getElementsByClassName("listImg");
				that.lazyload(); //页面载入完毕加载可是区域内的图片！ 
				// 滚动事件监听
				window.addEventListener('scroll', that.throttle(that.lazyload, 250, 500));
			}, 500)
		},
	}
</script>

<style>
	.list{
		display: flex;flex-direction: row;align-items: center;justify-content: space-between;
		padding: 10px;border: solid 1px #f2f2f2;margin-bottom:10px ;
		font-size: 12px;
	}
	.list-title{
		
	}
	.listImg {
		display: block;
		width: 100px;
		height: 100px;
	}
	.list-bottom{
		width: 100vw;
		color: #666;
		text-align: center;
		padding: 10px 0;
	}
</style>
