<style>
	.list {
		display: flex;
		flex-direction: row;
		align-items: center;
		white-space: nowrap;
		overflow: hidden;
		float: left;
		transform: translateX(0px);
	}

	.list-item {
		height: 40px;
		width: 100px;
		border-bottom: solid 1px red;
		text-align: center;
		line-height: 40px;
		font-size: 12px;
		position: relative;
	}

	.list-border-bottom {
		position: absolute;
		bottom: 0;
		left: 0;
		border-bottom: solid 1px #000000;
		width: 100px;
		transition: 0.3s ease;
	}

	.container-wrap {
		width: 100vw;
		white-space: nowrap;
		overflow: hidden;
		overflow-x: scroll;
		-webkit-backface-visibility: hidden;
		-webkit-perspective: 1000;
		-webkit-overflow-scrolling: touch;
	}

	.container-wrap::-webkit-scrollbar {
		display: none;
	}
	.add-inpuf {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		padding: 90px 0 40px 0;
		background: #fff;
	}
	
	.add-inpuf input {
		border: solid 1px #535353;
		border-radius: 5px;
		padding: 5px 10px;
		width: 75%;
		font-size: 12px
	}
	
	.add-inpuf view {
		padding-left: 10px;
		color: red
	}
</style>
<template>
	<view>
		<view class="container-wrap" id="box">
			<view class="list" >
				<view @click="chenTap(index)" class="list-item" v-for="(item,index) in arr" :key='index'>{{item}}</view>
				<view class="list-border-bottom" :style="'transform:translateX('+bottom_x+'px)'"></view>
			</view>
		</view>
		<view class="add-inpuf">
			<input  style='border:solid 1px #f2f2f2' type="text" v-model="typeName" placeholder="输入类目名称" />
			<view @click="addType">新增</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				typeName:'',
				bottom_x: 0,
				arr: ['汽车', '军事', '头条', '国际', '国内', '文学']
			};
		},
		methods: {
			chenTap(index) {
				this.bottom_x = index * 100;
			},
			addType(){
				var that=this;
				if(this.typeName){
					this.arr.push(this.typeName);
					setTimeout(function(){
						var ele = document.getElementById('box');
						ele.scrollLeft = that.arr.length*100
						that.bottom_x =( that.arr.length-1) * 100;
					},300)
				}
			}
		},
	}
</script>
