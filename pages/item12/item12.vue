<template>
	<view>
		<button @click="logTap">登录授权</button>
		<button open-type="getUserInfo" @getuserinfo='getuserinfo'>小程序登录授权</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods: {
			getuserinfo(res){
				console.info(res)
			},
			logTap() {
				let that=this;
				uni.login({
				provider: 'weixin',
				success: function (loginRes) {
				console.log(loginRes);
				// 获取用户信息
				uni.getUserInfo({
				provider: 'weixin',
				success: function (infoRes) {
				console.log(infoRes);
				// #ifdef MP-WEIXIN
				that.getOpenId(loginRes.code,infoRes.encryptedData,infoRes.iv)
				// #endif
				}
				});
				}
				});
			},
			getOpenId(code,encryptedData,iv){
				console.info(code)
					uni.request({
					url: 'https://api.weixin.qq.com/sns/jscode2session',
					data: {
					js_code: code,
					appid:'wx55ba219cf6074e1c',
					secret:'2e8160ec26b638f84f89fe61f699f823',
					grant_type:'authorization_code',
					encryptedData: encryptedData,
					iv: iv
					},
					success: (res) => {
					console.log(res);
					}
					});
			}
		},
	}
</script>

<style lang="less">

</style>
