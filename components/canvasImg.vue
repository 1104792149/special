<template>
	<view class="canvas">
		<canvas canvas-id="myCanvas"   :style="'width:'+imgValue.width+'px;height:'+imgValue.height+'px;border: 1px solid orangered;'"/> 
		<button type="primary"  @click="canvasTap()">开始</button>
	</view>
</template>

<script>
	export default {
		props:['imgValue'],
		data() {
			return {
				ctx:''
			};
		},
		computed:{
			
		},
		methods:{
			canvasTap(){
				 this.ctx = uni.createCanvasContext('myCanvas');
				 this.ctx.beginPath()
				this.imgValue.views.forEach((item,index)=>{
					switch (item.type){
						//矩形 radius
						case 'rect':
							this.canvasBugTap(item)
						break;
						//图片
						case 'image':	
							this.canvasImgTap(item)
						break;
						//圆形图片
						case 'radius':	
							this.canvasRyTap(item)
						break;
					};
				});
				//告诉 <canvas/> 组件你要将刚刚的描述绘制上去：
				this.ctx.draw(false);
			},
			//矩形
			canvasBugTap(value){
				console.info(value)
				//设置绘图上下文的填充色为红色：
				this.ctx.setFillStyle(value.background)
				//用 fillRect(x, y, width, height) 方法画一个矩形，填充为刚刚设置的红色：
				this.ctx.fillRect(value.x, value.y, value.width, value.height);
			},
			//画图片
			canvasImgTap(value){
				//drawImage(sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
				//dx, dy, dWidth, dHeight ，裁剪的区域和坐标可要可不要
				//图片路径，x轴坐标 y轴坐标 长 高
				this.ctx.drawImage(value.url, value.x, value.y, value.width, value.height)
			},
			//画圆形图片
			canvasRyTap(value){
				
				let left=80;
				let top=50;
				let height=50;
				let width=50;
				let borderRadius=width/2;
				//保存当前画布区域
				this.ctx.save()
				//裁剪 可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法）。
				this.ctx.arc(left + borderRadius, top + borderRadius, borderRadius, 0, 2 * Math.PI)
				this.ctx.clip()
				this.ctx.drawImage(value.url, left, top, width, height)
			},
			//画文字
			canvasTextTap(){
				// text	String	在画布上输出的文本
				// x	Number	绘制文本的左上角x坐标位置
				// y	Number	绘制文本的左上角y坐标位置
				// maxWidth	Number	需要绘制的最大宽度，可选
				const ctx = uni.createCanvasContext('myCanvas')
				ctx.setFontSize(20)
				ctx.setTextAlign('left')
				ctx.fillText('你是谁啊', 180, 60)
			
				// 水平对齐 可选值 'top'、'bottom'、'middle'、'normal'
				ctx.setTextBaseline('middle')
			
				ctx.draw()
			},
			opTap(){
				const ctx = uni.createCanvasContext('myCanvas')
				//设置绘图上下文的填充色为红色：
				ctx.setFillStyle('red')
				//用 fillRect(x, y, width, height) 方法画一个矩形，填充为刚刚设置的红色：
				ctx.fillRect(10, 10, 150, 75)
				ctx.setGlobalAlpha(0.2)
				//设置绘图上下文的填充色为红色：
				ctx.setFillStyle('blue')
				//用 fillRect(x, y, width, height) 方法画一个矩形，填充为刚刚设置的红色：
				ctx.fillRect(50, 50, 150, 75)
				ctx.draw()
			}	,
			addImg(){
				let that=this
				uni.canvasToTempFilePath({
				x: 0,
				y: 0,
				width: 300,
				height: 300,
				canvasId: 'myCanvas',
				success: function(res) {
				console.log(res)
				that.imgURL=res.tempFilePath
				// #ifdef H5
				return '';
				// #endif
				that.bcImg(res.tempFilePath);
				} 
				})
			},
			bcImg(url){
				uni.saveImageToPhotosAlbum({
				filePath:url,
				success: function () {
				console.log('save success');
				}
				});
			},
			_onLoad(){
				 this.ctx = uni.createCanvasContext('myCanvas');
			}
		}
	}
</script>

<style scoped lang="less">
.canvas{
	width: 99vw;
	display: flex;
	flex-direction: column;
	align-items: center;
	
}
</style>
