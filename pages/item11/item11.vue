<template>
	<view>
		<canvasImg :imgValue='pinImg'></canvasImg>
	</view>
</template>

<script>
	import canvasImg from '../../components/canvasImg.vue'
	export default {
		data() {
			return {
				canvasParemis:'',
				imgUrl:'../../static/3.png'
			};
		},
		components: {
			canvasImg
		},
		computed:{
			// 拼接图片内容
			pinImg(){
				var img={
				    width: 370,
				    height: 478,
				    views: [
				      {
						//矩形
				        type:'rect',background:'#fff',x:0,y: 0,width: 370,height: 478
				      },
				      {
				        type: 'image',url: this.imgUrl,x: 150,
				        y: 147.5,width: 80,height: 80
				      },
				     
				      {
				        type: 'text',content: '超级魂斗罗',fontSize: 20,color: '#000',textAlign: 'center',
				        top: 240, left: 187.5, width: 375,bolder: false
				      },
				     
				      {
						//圆形图片
				        type: 'radius',url:  this.imgUrl,
				        x: 100,y: 117.5, width: 70, height: 70,borderRadius:70
				      },
				      {
				     	  type: 'text',content: '长按识别二维码进入小游戏~',fontSize: 14,color: '#c0bab9',textAlign: 'center',top: 450,
				        left: 187.5, width: 375, lineHeight: 20,MaxLineNumber: 2,breakWord: true
				      }        
				    ]
				}
				return img
			}
		},
		methods:{
			
		}
	}
</script>

<style lang="less">

</style>
