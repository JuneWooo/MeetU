<template>
	<!-- 列表样式 -->
	<view class="p-2 animated fast fadeIn">
		<!-- 头像昵称| 关注按钮-->
		<view class="flex align-center justify-between" >
			<view class="flex align-center"> 
				<!-- 头像 -->
				<image class="rounded-circle mr-2" 
				:src="item.userpic" @click="openSpace(user_id)" 
				style="width: 65rpx;height: 65rpx;" 
				lazy-load></image>		
				<!-- 昵称发布时间 -->
				<view>
					<view class="font" style="line-height: 1.5;">{{item.username}}</view>
					<text class="font-sm text-light-muted" 
					style="line-height: 1.5;">{{item.newstime}}</text>
				</view>
			</view>
			
			<!-- 按钮 -->
			<view @click="follow" v-if="!item.isFollow && user_id!== item.user_id"
			class="flex align-center justify-center rounded bg-main text-white animated" 
			style="width: 90rpx; height: 50rpx;"
			hover-class="heartBeat">
				关注	
			</view>
			
		</view>
		<!-- 标题 -->
		<view class="font-md my-1" @click="openDetail">{{item.title}}</view>
		<!-- 图片 -->
		<image v-if="item.titlepic" :src="item.titlepic" 
		style="height: 350rpx;"
		class="rounded w-100"></image>
		<!-- 图标按钮 -->
		<view class="flex align-center">
			<!-- 顶 -->
			<view class="flex align-center justify-center flex-1 animated faster"
			hover-class="jello text-main" @click="doSupport('support')"
			:class="item.support.type === 'support' ? 'text-main':''">
				<text class="iconfont icon-dianzan1 mr-2"></text>
				<text>{{item.support.support_count > 0?item.support.support_count:'赞同'}}</text>
			</view>
			<!-- 踩 -->
			<view class="flex align-center justify-center flex-1 animated faster" 
			hover-class="jello text-main" @click="doSupport('unsupport')"
			:class="item.support.type === 'unsupport' ? 'text-main':''">
				<text class="iconfont icon-kulian mr-2"></text>
				<text>{{item.support.unsupport_count > 0?item.support.unsupport_count:'反对'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1 animated faster" 
			hover-class="jello text-main" @click="doComment">
				<text class="iconfont icon-xiaoxi mr-2"></text>
				<text>{{item.comment_count > 0?item.comment_count:'评论'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1 animated faster" 
			hover-class="jello text-main" @click="doShare">
				<text class="iconfont icon-fenxiang mr-2"></text>
				<text>{{item.share_num > 0?item.share_num:"分享"}}</text>
			</view>
		</view>
	
	</view>
</template>

<script>
	export default{
		props:{
			item: Object	,
			index: Number
		},
		methods:{
			//打开个人空间
			openSpace(user_id){
				// console.log('打开个人空间')
				uni.navigateTo({
					url: '/pages/user-space/user-space?user_id='+ user_id
				});
			},
			//关注
			follow(){
				// emit通知父组件
				// this.$emit('follow', this.index)
				this.checkAuth(()=>{
					this.$H.post('/follow', {
						follow_id:this.item.user_id
					},{
						token:true
					}).then(res=>{
						//通知更新
						uni.$emit('updateFollowOrSupport',{
							type:"follow",
							data:{
								user_id: this.item.user_id
							}
						})
					})
				})
			},
			//进入详情页
			openDetail(){
				console.log('进入详情页')
			},
			doComment(){
				console.log('评论接口')
			},
			doShare (){
				console.log('分享接口')
			},
			//顶踩操作
			doSupport(type){
				//通知父组件
				this.$emit('doSupport', {
					type: type,
					index: this.index
				})
			}
		}
	}
</script>

<style>
</style>
