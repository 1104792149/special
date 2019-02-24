<template>
	<view>
		<canvasImg></canvasImg>
	</view>
</template>

<script>
	import canvasImg from '../../components/canvasImg.vue'
	export default {
		data() {
			return {
				canvasParemis:'',
				imgUrl:'../../static/2.png'
			};
		},
		components: {
			canvasImg
		},
		methods:{
			// 拼接图片内容
			pinImg(){
			  var that=this       
				var img={
			      width: 375,
			      height: 578,
			      views: [
			        {
						//矩形
			          type:'rect',background:'#fff',top:120,left: 0,width: 375,height: 375
			        },
			        {
			          type: 'image',url: that.imgUrl,top: 150,
			          left: 147.5,width: 80,height: 80
			        },
			       
			        {
			          type: 'text',content: '超级魂斗罗',fontSize: 20,color: '#000',textAlign: 'center',
			          top: 240, left: 187.5, width: 375,bolder: false
			        },
			       
			        {
						//圆形图片
			          type: 'Radius',url:  that.imgUrl,
			          top: 300,left: 117.5, width: 70, height: 70,borderRadius:70
			        },
			        {
			       	  type: 'text',content: '长按识别二维码进入小游戏~',fontSize: 14,color: '#c0bab9',textAlign: 'center',top: 450,
			          left: 187.5, width: 375, lineHeight: 20,MaxLineNumber: 2,breakWord: true
			        }        
			      ]
			  }
			  return img
			}
		}
	}
</script>

<style lang="less">

</style>
