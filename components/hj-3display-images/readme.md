## 一个炫酷的3D图片展示模块
### 标签名: ```<hj-3display-images>```

### 属性： 
>#### 图片列表：images: Array<String>
>#### 是否横图显示（默认：true）：vertical: Boolean 
>#### 是否支持手势切换（默认：true）： vtouch: Boolean 
### 事件：
>#### @tap handler第一个参数是图片在列表中的index
### 已测试：微信小程序，安卓APP，H5(初步测试)
### 
##### v1.0.0 分享一个炫酷的图片展示模块吧！花了不少时间做的，主要是为了支持触摸 2019-01-26
### 使用示例
```
<template>
	<view class="content">
		<hj3-display-images :images="img" :vertical="false" :vtouch="false" @tap="tap"></hj3-display-images>
		 <view class="" >
		 	当前图片index:{{current}}
		 </view>
	</view>
</template>

<script>
	
	import hj3DisplayImages from '../../components/hj-3display-images/hj-3display-images.vue';
	export default {
		components:{
			hj3DisplayImages
		},
		data() {
			return {
				img: ['http://image.zhangxinxu.com/image/study/s/s128/mm1.jpg', 'http://image.zhangxinxu.com/image/study/s/s128/mm8.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm3.jpg', 'http://image.zhangxinxu.com/image/study/s/s128/mm4.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm6.jpg', 'http://image.zhangxinxu.com/image/study/s/s128/mm7.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm10.jpg', 'http://image.zhangxinxu.com/image/study/s/s128/mm13.jpg'
				],
				current:0
			};
		},
		methods: {
			tap:function (e) {
				this.current = e 
			}
		}
	}
</script>

<style scoped="">
	.content{
		display: flex;
		flex-direction: column;
	}
	
</style>

```