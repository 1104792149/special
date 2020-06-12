<style>
button {
	margin-top: 30upx;
	background-color: red;
	color: #fff;
}
</style>
<template>
	<view>
		<button @click="tap(1, '抽屉')">抽屉效果</button>
		<button @click="tap(2, '上下弹窗')">上下弹窗效果</button>
		<button @click="tap(3, '缩放弹窗')">缩放弹窗效果</button>
		<button @click="tap(4, '公告')">公告效果</button>
		<button @click="tap(5, '左右滚动')">左右滚动效果</button>
		<button @click="tap(6, '转圈上下浮动')">转圈 上下浮动 效果</button>
		<button @click="tap(7, '抽奖转盘')">抽奖转盘</button>
		<button @click="tap(8, '红包雨')">红包雨</button>
		<button @click="tap(9, '3d相册')">3d相册</button>
		<button @click="tap(10, 'canvas绘画')">canvas绘画</button>
		<button @click="tap(11, 'canvas绘制海报')">canvas绘制海报</button>
		<button @click="tap(12, '登录授权')">登录授权</button>
		<button @click="tap(13, '滑动抽屉')">滑动抽屉</button>
		<button @click="tap(14, '测试滑动抽屉')">测试滑动抽屉</button>
		<button @click="tap(15, '滑动导航栏')">滑动导航栏</button>
		<button @click="tap(16, '图片懒加载')">图片懒加载</button>
		<button @click="tap(17, '加强型滑动导航栏')">加强型滑动导航栏</button>
		<button @click="tap(18, '数字签名')">数字签名</button>
		<button @click="tap(19, '吸顶布局')">吸顶布局</button>
	</view>
</template>

<script>
export default {
	data() {
		return {};
	},
	onLoad() {
		var value = this.toChinese(1030);
		console.info(value);
	},
	methods: {
		tap(type, title) {
			uni.navigateTo({
				url: `../item${type}/item${type}?title=${title}+item${type}`
			});
		},

		// 精度只限于正整数（后期再优化）
		toChinese: function(num) {
			var num = parseInt(num);
			var overWan = Math.floor(num / 10000);
			var noWan = num % 10000;
			if (noWan.toString().length < 4) noWan = '0' + noWan;
			return overWan ? this.getWan(overWan) + '万' + this.getWan(noWan) : this.getWan(num);
		},
		getWan:function(temp){
			var changeNum = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九']; //changeNum[0] = "零"
			var unit = ['', '十', '百', '千', '万'];
			var strArr = temp.toString().split('').reverse();
			var newNum = '';
			for (var i = 0; i < strArr.length; i++) {
				newNum =(i == 0 && strArr[i] == 0 ? '' : i > 0 && strArr[i] == 0 && strArr[i - 1] == 0 ? '' : changeNum[strArr[i]] + (strArr[i] == 0 ? unit[0] : unit[i])) + newNum;
			}
			return newNum;
		}
	},
	watch: {}
};
</script>
