<template>
	<view>
		<!-- 顶部选项卡 -->
		<scroll-view scroll-x :scroll-into-view="scrollInto" scroll-with-animation
		class="scroll-row border-bottom border-light-secondary" 
		style="height: 100rpx;">
			<view v-for="(item,index) in tabBars" :key="index" 
			class="scroll-row-item px-3 py-2 font-md" :id="'tab'+index"
			:class="tabIndex === index?'text-main font-lg font-weight-bold': ''"
			@click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab"
		:style="'height:'+scrollH+'px;'">
			<swiper-item v-for="(item,index) in newslist" :key="index">
				<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'"
				@scrolltolower="loadmore(index)">
				<!-- 有数据 -->
				<template v-if="item.list.length > 0">
					<!-- 列表 -->
					<block v-for="(item2,index2) in item.list" :key="index2">
						<!-- 列表样式 -->
						<common-list :item="item2" :index="index2" @follow="follow" @doSupport="doSupport"></common-list>
						<!-- 全局分割线 -->
						<divider></divider>
					</block>
					<!-- 上拉加载 -->
					<load-more :loadmore="item.loadmore"></load-more>
				</template>
				
				<!-- 无数据 -->
				<template v-else>
					<no-thing></no-thing>
				</template>
				
				</scroll-view>
			</swiper-item>
			
		</swiper>
	</view>
</template>

<script>
	const demo = [{
		username: "吴骏恩",
		userpic:"/static/default.jpg",
		newstime:"2020-5-18 下午12:11",
		isFollow:false,
		title:"这里是标题",
		titlepic:"/static/demo.jpg",
		support:{
			type:"support",//顶
			support_count:1.,
			unsupport_count:0
		},
		comment_count:2,
		share_num:2
	},
	{
		username: "骏恩",
		userpic:"/static/default.jpg",
		newstime:"2020-5-18 下午12:11",
		isFollow:false,
		title:"这里是标题",
		titlepic:"",
		support:{
			type:"unsupport", //踩
			support_count:0,
			unsupport_count:2
		},
		comment_count:0,
		share_num:2
	},
	{
		username: "恩",
		userpic:"/static/default.jpg",
		newstime:"2020-5-18 下午12:11",
		isFollow:false,
		title:"这里是标题",
		titlepic:"/static/demo.jpg",
		support:{
			type:"", //未操作
			support_count:1.,
			unsupport_count:2
		},
		comment_count:2,
		share_num:0
	}]
	
	// “@/” 代表从根目录开始
	import commonList from '@/components/common/common-list.vue';
	import loadMore from '@/components/common/load-more.vue';
	export default {
		components:{
			commonList,
			loadMore
		},
		data() {			
			return {
				//列表高度
				scrollH:600,
				// 顶部选项卡
				scrollInto:'',
				tabIndex:0,
				tabBars:[
					{name:'关注'},
					{name:'推荐'},
					{name:'同城'},
					{name:'热点'},
					{name:'历史'},
					{name:'财经'},
					{name:'科技'},
					{name:'人文'},
				],
				newslist:[]
			}
		},
		onLoad(){
			uni.getSystemInfo({
				success:res=>{
					this.scrollH = res.windowHeight - uni.upx2px(101)
				}
			})
			// 根据选项生成列表
			this.getData()
		},
		methods :{
			//获取数据
			getData(){
				var arr = []
				for (let i=0; i<this.tabBars.length; i++){
					// 生成列表模板
					let obj = {
						// 1.上拉加载更多  2.加载中...  3.没有更多了
						loadmore:"上拉加载更多",
						list:[]
					}
					arr.push(obj)
					if (i < 2){
						obj.list = demo
					}
				}
				this.newslist = arr
			},
			//监听滑动
			onChangeTab(e){
				this.changeTab(e.detail.current)
			},
			// 切换选项
			changeTab(index){
				if (this.tabIndex === index){
					return;
				}
				this.tabIndex = index
				// 滚动到指定元素
				this.scrollInto = 'tab'+index
			},
			//关注
			follow(e){
				this.newslist[this.tabIndex].list.forEach(iteme=>{
					if(item.id === e.id){
						item.list[e].isFollow = true
						uni.showToast({'title': '关注成功！'})
					}
				})			
			},
			//顶踩
			doSupport(e){
				// 拿到当前选项卡的对应list
				this.newslist[this.tabIndex].list.forEach(item=>{
					if(item.id === e.id){
						// 之前没有操作过
						if (item.support.type ==='') {
							item.support[e.type+'_count'] ++
						}//之前顶了
						else if (item.support.type === 'support' && e.type === 'unsupport'){
							item.support.support_count--; //顶-1，
							item.support.unsupport_count++; //踩+1
						}//之前踩了
						else if(item.support.type === 'unsupport' && e.type === 'support'){
							item.support.support_count++; //顶 +1
							item.support.unsupport_count--;//踩 -1
						}
						
					}
					item.support.type = e.type
				})
				let msg = e.type === 'support' ? '顶':'踩'
				uni.showToast({title:msg+'成功'});	
			},
			// 上拉加载更多
			loadmore(index){
				// 拿到当前列表
				let item = this.newslist[index]	
				//判断是否处于可加载状态(上拉加载更多)
				if(item.loadmore !== '上拉加载更多') return;
				//修改当前列表加载状态
				item.loadmore = "加载中..."
				// console.log('上拉加载更多')
				// 模拟数据请求
				setTimeout(()=>{
					//加载数据
					item.list = [...item.list, ...item.list]
					//回复加载状态
					item.loadmore = '上拉加载更多'
				},2000) 
			}
		}
	}
</script>

<style>

</style>
