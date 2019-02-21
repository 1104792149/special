<template>
	<view style="display: flex;flex-direction: column;align-items: center;width: 100vw;margin-top: 20upx;">
		<canvas canvas-id="myCanvas"  style="border: 1px solid;width: 300px;height: 300px;"/>
		<button type="primary"  @click="canvasBugTap">绘制背景色</button>
		<button type="primary" @click="canvasImgTap">绘制图片</button>
		<button type="primary" @click="canvasRyTap">绘制圆形图片</button>
		<button type="primary" @click="canvasTextTap">绘制文字</button>
		<button type="primary" @click="opTap">绘制自定义透明度背景，图</button>
		<button type="primary" @click="addImg">保存图片</button>
		<image :src="imgURL"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgURL:''
			};
		},
		methods:{
			canvasBugTap(){
				//创建上下文
				const ctx = uni.createCanvasContext('myCanvas')
				//设置绘图上下文的填充色为红色：
				ctx.setFillStyle('red')
				//用 fillRect(x, y, width, height) 方法画一个矩形，填充为刚刚设置的红色：
				ctx.fillRect(10, 10, 150, 75)
				//告诉 <canvas/> 组件你要将刚刚的描述绘制上去：
				ctx.draw()
			},
			//画图片
			canvasImgTap(){
				//drawImage(sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
				//dx, dy, dWidth, dHeight ，裁剪的区域和坐标可要可不要
				//创建上下文
				const ctx = uni.createCanvasContext('myCanvas')
				//选择图片，
				uni.chooseImage({
					success: function(res){
						//图片路径，x轴坐标 y轴坐标 长 高
						ctx.drawImage(res.tempFilePaths[0], 0, 0, 150, 100)
						ctx.draw()
					}
				})
			},
			//画圆形图片
			canvasRyTap(){
				const context = uni.createCanvasContext('myCanvas');
				let left=80;
				let top=50;
				let height=50;
				let width=50;
				let borderRadius=width/2;
				//选择图片，
				uni.chooseImage({
					success: function(res){
						//保存当前画布区域
						context.save()
						context.beginPath()
						//裁剪 可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法）。
						context.arc(left + borderRadius, top + borderRadius, borderRadius, 0, 2 * Math.PI)
						context.clip()
						context.drawImage(res.tempFilePaths[0], left, top, width, height)
						//<canvas/> 组件你要将刚刚的描述绘制上去：
						context.draw()
					}
				})
				
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
			}
		}
	}
</script>

<style lang="less">
button{
	margin-top: 20upx;
}
</style>
