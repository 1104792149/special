<template>
	<view >
		<view class="canvas">
			<canvas canvas-id="myCanvas"   :style="'width:'+imgValue.width+'px;height:'+imgValue.height+'px;'"/> 
		</view>
		<button style="margin-top: 20upx;" type="primary"  @click="canvasTap()">开始</button>
		<button style="margin-top: 20upx;" type="primary"  @click="addImg()">生成图片</button>
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
				uni.showLoading({
					title: '生成中...'
				});
				// this.ctx.setGlobalAlpha(0.2);透明属性
				this.ctx = uni.createCanvasContext('myCanvas');
				this.imgValue.views.forEach((item,index)=>{
					this.ctx.save(); 
					this.ctx.beginPath(); 
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
						case 'text':	
							this.drawText(item)
						break;
					};
				});
				//告诉 <canvas/> 组件你要将刚刚的描述绘制上去：
				this.ctx.draw(false);
				uni.hideLoading();
			},
			//矩形
			canvasBugTap(params){
				//设置绘图上下文的填充色为红色：
				this.ctx.setFillStyle(params.background)
				//用 fillRect(x, y, width, height) 方法画一个矩形，填充为刚刚设置的红色：
				this.ctx.fillRect(params.x, params.y, params.width, params.height);
				this.ctx.setStrokeStyle('#000')
				this.ctx.strokeRect(0, 0, this.imgValue.width, this.imgValue.height)
			},
			//画图片
			canvasImgTap(params){
				//drawImage(sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
				//dx, dy, dWidth, dHeight ，裁剪的区域和坐标可要可不要
				//图片路径，x轴坐标 y轴坐标 长 高
				this.ctx.drawImage(params.url, params.x, params.y, params.width, params.height)
			},
			//画圆形图片
			canvasRyTap(params){
				const { url, y = 0, x = 0, width = 0, height = 0, borderRadius = 0 } = params;
				let d = borderRadius * 2;
				let cx = x + borderRadius; let cy = y + borderRadius;
				this.ctx.save(); 
				this.ctx.beginPath(); 
				this.ctx.arc(cx, cy, borderRadius, 0, 2 * Math.PI);
				this.ctx.fill();
				this.ctx.clip(); 
				this.ctx.drawImage(url, x, y, d, d);
				this.ctx.restore();
			},
			//画文字
			 drawText (params) {
			  this.ctx.save()
			  const {
			    MaxLineNumber = 2,
			    breakWord = false,
			    color = 'black',
			    content = '',
			    fontSize = 16,
			    y = 0,
			    x = 0,
			    lineHeight = 20,
			    textAlign = 'left',
			    width,
			    textDecoration = 'none'
			  } = params
			  
			  this.ctx.beginPath()
			  this.ctx.setTextBaseline('top')
			  this.ctx.setTextAlign(textAlign)
			  this.ctx.setFillStyle(color)
			  this.ctx.setFontSize(fontSize)
			
			  if (!breakWord) {
			    this.ctx.fillText(content, x, y)
			    this.drawTextLine(x, y, textDecoration, color, fontSize, content)
			  } else {
			    let fillText = ''
			    let fillTop = y
			    let lineNum = 1
			    for (let i = 0; i < content.length; i++) {
			      fillText += [content[i]]
			      if (this.ctx.measureText(fillText).width > width) {
			        if (lineNum === MaxLineNumber) {
			          if (i !== content.length) {
			            fillText = fillText.substring(0, fillText.length - 1) + '...'
			            this.ctx.fillText(fillText, x, fillTop)
			            this.drawTextLine(x, fillTop, textDecoration, color, fontSize, fillText)
			            fillText = ''
			            break
			          }
			        }
			        this.ctx.fillText(fillText, x, fillTop)
			        this.drawTextLine(x, fillTop, textDecoration, color, fontSize, fillText)
			        fillText = ''
			        fillTop += lineHeight
			        lineNum ++
			      }
			    }
			    this.ctx.fillText(fillText, x, fillTop)
			    this.drawTextLine(x, fillTop, textDecoration, color, fontSize, fillText)
			  }
			  
			  this.ctx.restore()
			},
			drawTextLine (x, y, textDecoration, color, fontSize, content) {
			  if (textDecoration === 'underline') {
			    this.drawRect({
			      background: color,
			      y: y + fontSize * 1.2,
			      x: x - 1,
			      width: this.ctx.measureText(content).width + 3,
			      height: 1
			    })
			  } else if (textDecoration === 'line-through') {
			    this.drawRect({
			      background: color,
			      y: y + fontSize * 0.6,
			      x: x - 1,
			      width: this.ctx.measureText(content).width + 3,
			      height: 1
			    })
			  }
			},
			addImg(){
				uni.showLoading({
					title: '保存中...'
				});
				//保存图片
				let that=this
				uni.canvasToTempFilePath({
				x: 0,
				y: 0,
				width: that.imgValue.width,
				height: that.imgValue.height,
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
					uni.showToast({
						title:'保存图片成功'
					})
				uni.hideLoading();
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
