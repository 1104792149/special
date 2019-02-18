<style scoped lang="less">
.dialog-content {
        position: fixed;
		top: 50%;
		width: 100vw;
		display: flex;flex-direction: column;align-items: center;
        transition: 0.3s ease;
        transform: translateY(-50%) scale(0.2);
        opacity: 0;
        &.show {
            opacity: 1;
            transform: translateY(-50%) scale(1);
        }

    }
</style>

<template>
	<view>
		<button @click="tap">缩放弹窗</button>
		 <view class="dialog-content" :class="{ show: isShow }">
			<view style="width: 200upx;height: 200upx;background: #FF0000;"></view>
        </view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isShow:false
			};
		},
		methods:{
			tap(){
				this.isShow=!this.isShow;
			}
		}
	}
</script>

