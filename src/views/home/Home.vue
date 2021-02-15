<template>
  <div id="home">
	<nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
<!-- 	<swiper>
		<swiper-item v-for="(item,index) in banners" :key="index">
          <a :href="item.link">
			  <img :src="item.image" alt="">
			  </a>
			  
		</swiper-item> -->
		<scroll class="content" ref="scroll" :probe-type="3"
		              @scroll="contentScroll"
					  @pullingUp="loadmore"
		               :pull-up-load="true">
			<home-swiper :banners="banners" @swiperImageLoad="swiperImageLoa"></home-swiper>
			<recommend-view :recommends="recommends"></recommend-view>
			<feature-vew></feature-vew>
			<tab-control :titles="['流行','新款','精选']"  @tabClick="tabClick" ref="tabControl"></tab-control>
			<good-list :goods="showgoodsList"></good-list>
		</scroll>
			<backtop @click.native="backClick" v-show="isShowBackTop"></backtop>
	</swiper>
  </div>
</template>

<script>
	import NavBar from 'components/common/navbar/Naxbar';
	import TabControl from '../../components/content/tabControl/TabControl';
	import HomeSwiper from './childComps/HomeSwiper';
	import RecommendView from './childComps/RecommendView';
	import FeatureVew from './childComps/FeatureView';
	import GoodList from 'components/content/goods/GoodList';
	import Scroll from 'components/common/scroll/Scroll';
	import Backtop from 'components/content/backtop/Backtop';
	// import {getHomeMultidata} from 'network/home';
	// import {Swiper,SwiperItem} from 'components/common/swiper'
	import {getHomeMultidata,getHomeGoods} from "../../network/home";
	
  export default {
    name: "Home",
	components:{
		NavBar,
		HomeSwiper,
	    RecommendView,
		FeatureVew,
		TabControl,
		GoodList,
		Scroll,
		Backtop
		
	},
	data(){
		return{
		 banners:[],
		 recommends:[],
		 goods:{
			 'pop':{page:0,list:[]},
			 'new':{page:0,list:[]},
			 'sell':{page:0,list:[]},
		 },
		 currentType:'pop',
		 tabOffsetTop:0,
		 isShowBackTop:false,
		 
		 
		 }
	},
	 computed:{
		showgoodsList(){
			return this.goods[this.currentType].list;
		} 
	 },
	created(){
		//1.请求多个数据
		// getHomeMultidata().then(res=>{
		// 	//console.log(res);
		// 	//this.result=res;
		// 	this.banners=res.data.banner.list;
		// 	this.recommends=res.data.recommend.list;
			
		// }),
		this.getHomeMultidata();
		//2.请求商品数据
		// getHomeGoods('pop',1).then(res=>{
		// 	console.log(res);
		// })
		this.getHomeGoods('pop');
		this.getHomeGoods('new');
		this.getHomeGoods('sell');
		this.$bus.$on("itemImageLoad",()=>{
			// console.log("-----");
			this.$refs.scroll.refresh();
		})
	},
	mounted(){
		// this.tabOffsetTop=this.$refs.tabControl.offsetTop;
		 
	},
	methods:{
		/**
		 * 事件监听的相关方法
		 */
		   tabClick(index){
			   switch(index){
				   case 0:
				   this.currentType='pop'
				   break
				   case 1:
				   this.currentType='new'
				   break
				   case 2:
				   this.currentType='sell';
				   break
			   }
		   },
		   backClick(){
			   // this.$refs.scroll.scroll.scrollTo(0,0,500);
			   this.$refs.scroll.scrollTo(0,0)
		   },
		   swiperImageLoa(){
	         	this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop; 
		   },
		   contentScroll(position){
			 this.isShowBackTop=(-position.y)>1000;
			 
		   },
		   //上拉加载更多
		   loadmore(){
			 this.getHomeGoods(this.currentType);
		   },
		   
		   
		/**
		 * 网络请求
		 */
		getHomeMultidata(){
			getHomeMultidata().then(res=>{
				//console.log(res);
				//this.result=res;
				this.banners=res.data.banner.list;
				this.recommends=res.data.recommend.list;
				
			})
		},
		getHomeGoods(type){
			const page=this.goods[type].page+1;
			getHomeGoods(type,page).then(res=>{
				this.goods[type].list.push(...res.data.list);
				this.goods[type].page+=1;
			})
		}
	}
	
	
  }
</script>

<style scoped>
	#home{
		/* padding-top: 44px; */
		/* padding-bottom: 400px; */
		height: 100vh;
		
	}
 .home-nav{
	 background-color: var(--color-tint);
	 color: white;
	 position: fixed;
	 left: 0;
	 right: 0;
	 top: 0;
	 z-index: 9;
 }
/* .tab-control{
	 position: sticky;
	 top:44px;
	 z-index: 9;
	 background-color: #FFF0FF;
 } */
  .content{
	 height: calc(100% - 93px);
	 overflow: hidden;
	 margin-top: 44px;
 } 
</style>
