<template>
	<view>
		<!-- <view class="">
			消息页
		</view> -->
		<!-- <text :selectable="false"> 这里是一些文本\n 支持换行。。。</text> -->
		<!-- <view class="font">{{name}}</view> -->
		
		<!-- <button class="box" @tap="clickeven()">按钮</button> -->
		<view class="font">
			{{ZHweigth}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: "哈哈哈",
				// 体重
				weight: 2000
			}
		},
		computed:{
			ZHweigth:function(){
				return this.weight>1000 ? (this.weight/1000)+'kg':this.weight+'g'
			}
		},
		methods: {
			
			clickeven:function(){
				this.name = "嘻嘻嘻"
			}
		}
	}
</script>

<style>
.box{
	background: #0A98D5;
	color: #FF3333;
	width: 80%;
	margin: auto;
	height: 80upx;
	font-size: 30upx;
	border-radius: 30upx;
	border: 1upx solid #EEEEEE;
	display: flex;
	justify-content: center; 
	align-items: conter;
}
.font{
	font-size: 50upx;
	border: 1upx solid #CCCCCC;
	padding: 20upx;
}
</style>
