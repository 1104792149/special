<template>
	<view>
		<view class="ly-tabbar" ref="viewArea">
			<view class="ly-tab-list" :style="style" ref="list">
				<a class="ly-tab-item" :style="index === selectedId ? activeStyle : {}" @click="handleChange(item,index)" v-for="(item, index) in items"
				 :key="index" :class="[index==0?'':'ly-tab-item0']">
					<view class="ly-tab-item-label">
						{{item.label}}
					</view>
				</a>
				<view class="ly-tab-active-bar" :style="activeBarStyle" ref="activeBar"></view>
			</view>
		</view>
		<view class="add-inpuf">
			<input  style='border:solid 1px #f2f2f2' type="text" v-model="typeName" placeholder="输入类目名称" />
			<view @click="addType">新增</view>
		</view>
	</view>
</template>

<script>
// 	import {
// 		windowInit
// 	} from '../../js/requestAnimationFrame.js'
	export default {
		data() {
			return {
				viewAreaWidth:0,//可视区宽度
				listWidth:0,//可视区与可滑动元素宽度差值
				typeName:'',//新增类目名
				selectedId: 0, //当前选中的下标
				items: [{
						label: '首页'
					},
					{
						label: '推荐'
					},
					{
						label: 'Android'
					},
					{
						label: '前端'
					},
					{
						label: '后端'
					},
					{
						label: 'iOS'
					},
					{
						label: '产品'
					},
					{
						label: '人工智能'
					},
					{
						label: '设计'
					}
				],
				lineHeight: 3, //下标线高
				activeColor: '#1d98bd', //主题样色
				additionalX: 50, // 近似等于超出边界时最大可拖动距离(px);
				reBoundExponent: 10, // 惯性回弹指数(值越大，幅度越大，惯性回弹距离越长);
				sensitivity: 1000, // 灵敏度(惯性滑动时的灵敏度,值越小，阻力越大),可近似认为速度减为零所需的时间(ms);
				reBoundingDuration: 360, // 回弹过程duration;
				activeBarX: 0,
				activeBarWidth: 0,
				speed: 0, // 滑动速度(正常滑动时一般不会超过10);
				touching: false, // 是否处于touch状态;
				reBounding: false, // 是否处于回弹过程;
				translateX: 0,
				startX: 0,
				lastX: 0,
				currentX: 0,
				startMoveTime: 0,
				endMoveTime: 0,
				frameTime: 16.7, // 每个动画帧的ms数
				frameStartTime: 0,
				frameEndTime: 0,
				inertiaFrame: 0,
				zeroSpeed: 0.001, // 当speed绝对值小于该值时认为速度为0 (可用于控制惯性滚动结束期的顺滑度)
				acceleration: 0, // 惯性滑动加速度;
			};
		},
		watch: {

		},
		computed: {
			style() {
				return {
					transitionTimingFunction: this.transitionTimingFunction,//动画效果 规定以相同速度开始至结束的过渡效果 规定慢速开始，然后变快，然后慢速结束的过渡效果
					transitionDuration: `${this.transitionDuration}ms`,//动画时间
					transform: `translate3d(${this.translateX}px, 0px, 0px)`//动画属性
				}
			},
			activeBarStyle() {
				return {
					transition: `all 300ms`,
					width: `${this.activeBarWidth}px`,
					height: `${this.lineHeight}px`,
					transform: `translate3d(${this.activeBarX}px, 0, 0)`,
					backgroundColor: this.activeColor
				}
			},
			transitionDuration() {
				if (this.touching || (!this.reBounding && !this.touching)) {
					return 0
				}
				if (this.reBounding && !this.touching) {
					return this.reBoundingDuration
				}
			},
			transitionTimingFunction() {
				return this.reBounding ? 'cubic-bezier(0.25, 0.46, 0.45, 0.94)' : 'cubic-bezier(0.1, 0.57, 0.1, 1)'
			},
			// 是否向左惯性滚动;
			isMoveLeft() {
				return this.currentX <= this.startX
			},
			// 是否向右惯性滚动;
			isMoveRight() {
				return this.currentX >= this.startX
			},
			//选中的字体颜色
			activeStyle() {
				return {
					color: this.$parent.activeColor
				}
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});
			var that=this
			setTimeout(function(){
				that.bindEvent();//监听事件
				that.handleChange(that.items[that.selectedId].label,that.selectedId);//初始化导航位置
				that.getWidth();//可视区宽度; 可视区与可滑动元素宽度差值
				that.windowInit();//初始化定时器requestAnimationFrame
			},300)
		},
		destoryed() {
			this.removeEvent();//卸载事件监听
		},
		methods: {
			windowInit(){
				 var lastTime = 0
				var vendors = ['webkit', 'moz']
				for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
				  window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame']
				  window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] || // name has changed in Webkit
				                                window[vendors[x] + 'CancelRequestAnimationFrame']
				}
				
				if (!window.requestAnimationFrame) {
				  window.requestAnimationFrame = function (callback, element) {
				    var currTime = new Date().getTime()
				    var timeToCall = Math.max(0, 16.7 - (currTime - lastTime))
				    var interval = currTime - lastTime
				    var id = window.setTimeout(function () {
				      callback(interval)
				    }, timeToCall)
				    lastTime = currTime + timeToCall
				    return id
				  }
				}
				if (!window.cancelAnimationFrame) {
				  window.cancelAnimationFrame = function (id) {
				    clearTimeout(id)
				  }
				}
			},
			addType(){
				var that = this;
				if (this.typeName) {
					var obj={
						label:''
					};
					obj.label=this.typeName
					this.items.push(obj);
					setTimeout(function(){
						that.getWidth();//可视区宽度; 可视区与可滑动元素宽度差值
						that.handleChange(that.typeName,that.items.length - 1);
					},300)
				} else {
					H.$toast('类目不能为空');
				}
			},
			getWidth(){
				// 可视区宽度; 可视区与可滑动元素宽度差值
				var dom = this.$refs.viewArea.$el;
				this.viewAreaWidth=dom.offsetWidth;
				// 可视区与可滑动元素宽度差值;
				var dom0 = this.$refs.list.$el;
				this.listWidth=dom0.offsetWidth - this.viewAreaWidth;
			},
			handleChange(item, index) {
				console.log(item, index)
				this.selectedId = index;
				this.checkPosition()
				this.calcBarPosX()
			},
			// start
			handleTouchStart(event) {
				event.stopPropagation()
				cancelAnimationFrame(this.inertiaFrame)
				this.lastX = event.touches[0].clientX
			},

			// move
			handleTouchMove(event) {
				if (this.listWidth <= 0) return
				event.preventDefault()
				event.stopPropagation()
				this.touching = true
				this.startMoveTime = this.endMoveTime
				this.startX = this.lastX
				this.currentX = event.touches[0].clientX
				this.moveFollowTouch()
				this.endMoveTime = event.timeStamp // 每次触发touchmove事件的时间戳;
			},

			// end
			handleTouchEnd(event) {
				this.touching = false
				if (this.checkReboundX()) {
					cancelAnimationFrame(this.inertiaFrame)
				} else {
					let silenceTime = event.timeStamp - this.endMoveTime
					let timeStamp = this.endMoveTime - this.startMoveTime
					timeStamp = timeStamp > 0 ? timeStamp : 8
					if (silenceTime > 100) return // 停顿时间超过100ms不产生惯性滑动;
					this.speed = (this.lastX - this.startX) / timeStamp
					this.acceleration = this.speed / this.sensitivity
					this.frameStartTime = new Date().getTime()
					this.inertiaFrame = requestAnimationFrame(this.moveByInertia)
				}
			},

			// 如果需要回弹则进行回弹操作并返回true;
			checkReboundX() {
				this.reBounding = false
				if (this.translateX > 0) {
					this.reBounding = true
					this.translateX = 0
				} else if (this.translateX < -this.listWidth) {
					this.reBounding = true
					this.translateX = -this.listWidth
				}
				return this.translateX === 0 || this.translateX === -this.listWidth
			},

			bindEvent() {
				this.$refs.list.$el.addEventListener('touchstart', this.handleTouchStart, false)
				this.$refs.list.$el.addEventListener('touchmove', this.handleTouchMove, false)
				this.$refs.list.$el.addEventListener('touchend', this.handleTouchEnd, false)
			},

			removeEvent() {
				this.$refs.list.$el.removeEventListener('touchstart', this.handleTouchStart)
				this.$refs.list.$el.removeEventListener('touchmove', this.handleTouchMove)
				this.$refs.list.$el.removeEventListener('touchend', this.handleTouchEnd)
			},

			// touch拖动
			moveFollowTouch() {
				if (this.isMoveLeft) { // 向左拖动
					if (this.translateX <= 0 && this.translateX + this.listWidth > 0 || this.translateX > 0) {
						this.translateX += this.currentX - this.lastX
					} else if (this.translateX + this.listWidth <= 0) {
						this.translateX += this.additionalX * (this.currentX - this.lastX) /
							(this.viewAreaWidth + Math.abs(this.translateX + this.listWidth))
					}
				} else { // 向右拖动
					if (this.translateX >= 0) {
						this.translateX += this.additionalX * (this.currentX - this.lastX) /
							(this.viewAreaWidth + this.translateX)
					} else if ((this.translateX <= 0 && this.translateX + this.listWidth >= 0) ||
						this.translateX + this.listWidth <= 0) {
						this.translateX += this.currentX - this.lastX
					}
				}
				this.lastX = this.currentX
			},

			// 惯性滑动
			moveByInertia() {
				this.frameEndTime = new Date().getTime()
				this.frameTime = this.frameEndTime - this.frameStartTime
				if (this.isMoveLeft) { // 向左惯性滑动;
					if (this.translateX <= -this.listWidth) { // 超出边界的过程;
						// 加速度指数变化;
						this.acceleration *= (this.reBoundExponent +
								Math.abs(this.translateX + this.listWidth)) /
							this.reBoundExponent
						this.speed = Math.min(this.speed - this.acceleration, 0) // 为避免减速过程过短，此处加速度没有乘上frameTime;
					} else {
						this.speed = Math.min(this.speed - this.acceleration * this.frameTime, 0)
					}
				} else if (this.isMoveRight) { // 向右惯性滑动;
					if (this.translateX >= 0) {
						this.acceleration *= (this.reBoundExponent + this.translateX) / this.reBoundExponent
						this.speed = Math.max(this.speed - this.acceleration, 0)
					} else {
						this.speed = Math.max(this.speed - this.acceleration * this.frameTime, 0)
					}
				}
				this.translateX += this.speed * this.frameTime / 2
				if (Math.abs(this.speed) <= this.zeroSpeed) {
					this.checkReboundX()
					return
				}
				this.frameStartTime = this.frameEndTime
				this.inertiaFrame = requestAnimationFrame(this.moveByInertia)
			},

			// 计算activeBar的translateX this.selectedId children  var lastW = divs[i].children[1].offsetWidth; .$el
			calcBarPosX() {
				var childrenArr=this.screenItem();
				if (childrenArr.length <= this.selectedId) return
				const item = childrenArr[this.selectedId].$el
				const itemWidth = item.offsetWidth
				const itemLeft = item.offsetLeft
				this.activeBarWidth = Math.max(itemWidth * 0.6, 14)
				this.activeBarX = itemLeft + (itemWidth - this.activeBarWidth) / 2
			},
			//筛选list的item
			screenItem(){
				var arr = this.$refs.list.$children;
				var newArr=[];
				for (var i=0;i<arr.length;i++) {
					if('ly-tab-item-label'==arr[i].$el.className){
						newArr.push(arr[i]);
					};
				};
				return newArr;
			},
			// 点击切换item时，调整位置使当前item尽可能往中间显示
			checkPosition() {
				var childrenArr=this.screenItem();
				if (childrenArr.length <= this.selectedId) return
				const activeItem = childrenArr[this.selectedId].$el
				const offsetLeft = activeItem.offsetLeft
				const half = (this.viewAreaWidth - activeItem.offsetWidth) / 2
				let changeX = 0
				const absTransX = Math.abs(this.translateX)
				if (offsetLeft <= absTransX + half) { // item偏左，需要往右移
					let pageX = offsetLeft + this.translateX
					changeX = half - pageX
				} else { // item偏右，需要往左移
					changeX = -(offsetLeft - absTransX - half)
				}
				let lastX = changeX + this.translateX
				// 两种边界情况
				lastX > 0 && (lastX = 0)
				lastX < -this.listWidth && (lastX = -this.listWidth)
				this.reBounding = true
				this.translateX = lastX;
			}
		}

	}
</script>

<style scoped="scoped">
	.ly-tab-item {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		text-decoration: none;
		flex-grow: 1;
		font-size: 14px;
		text-align: center;
		padding: 0 5px;
	}

	.ly-tab-item-icon {
		margin: 0 auto 5px;
	}

	.ly-tabbar {
		position: relative;
		background-color: rgba(255, 255, 255, .8);
		width: 100%;
		overflow: hidden;
		display: flex;
		border-bottom: 1px solid #eee;
		box-shadow: 0 0px 6px 1px #eee
	}

	.ly-tabbar.ly-tabbar-fix-bottom {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		border-top: 1px solid #ccc;
		border-bottom: none;
	}

	.ly-tabbar.ly-tabbar-fix-bottom .ly-tab-item {
		border-bottom: none !important;
	}

	.ly-tab-list {
		position: relative;
		box-sizing: border-box;
		display: flex;
		flex-flow: row nowrap;
		flex-shrink: 0;
		padding: 14px 10px;
		min-width: 100%;
	}

	.ly-tab-active-bar {
		position: absolute;
		bottom: 3px;
		left: 0;
		width: 30px;
		height: 3px;
		border-radius: 4px;
	}

	.ly-tab-item0 {
		margin-left: 15px;
	}
	.add-inpuf {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		padding: 90px 0 40px 0;
		background: #fff;
	}
	
	.add-inpuf input {
		border: solid 1px #535353;
		border-radius: 5px;
		padding: 5px 10px;
		width: 75%;
		font-size: 12px
	}
	
	.add-inpuf view {
		padding-left: 10px;
		color: red
	}
</style>
