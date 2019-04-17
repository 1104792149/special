<template>
	<view class="content">
		<hj3-display-images ref='hj3' :images="img" :vertical="true"  :vtouch="true" @callbackTap1="callbackTap"></hj3-display-images>
		<view class="">
			当前图片index:{{current}}
		</view>
		<button @click="aaTap">自动轮播</button>
	</view>
</template>
<script>
	import hj3DisplayImages from '../../components/hj-3display-images/hj-3display-images.vue';
	export default {
		components: {
			hj3DisplayImages
		},
		data() {
			return {
				img: ['http://image.zhangxinxu.com/image/study/s/s128/mm1.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm8.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm3.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm4.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm6.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm7.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm10.jpg',
					'http://image.zhangxinxu.com/image/study/s/s128/mm13.jpg'
				],
				current: 0
			};
		},
		methods: {
			aaTap(){
				this.$refs.hj3.show();
			},
			callbackTap: function(e) {
				this.current = e
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: e.title
			});}
	}
</script>
<style scoped="scoped" lang="less">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		
	}
	</style>
