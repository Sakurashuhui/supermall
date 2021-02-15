<template>
 <!-- <h2>分类</h2> -->
 <div id="category">
 <nav-bar class="category-nav"><div slot="center">商品分类</div></nav-bar>


<scroll class="content" ref="scroll" :probe-type="3" :pull-up-load="true">
  <ul>
  	<li>正在流行</li>
	<li>上衣</li>
	<li>裤子</li>
	<li>裙子</li>
	<li>内衣</li>
	<li>女鞋</li>
	<li>男友</li>
	<li>包包</li>
	<li>运动</li>
	<li>配饰</li>
	<li>美妆</li>
	<li>个户</li>
  </ul>
  </Scroll>
 </div>


</template>

<script>
	import Scroll from 'components/common/scroll/Scroll';
	import NavBar from 'components/common/navbar/Naxbar';
  export default {
    name: "Category",
	components:{
		Scroll,
        NavBar
	},
	data(){
		return{
			scroll:null
		}
	},
	// created(){
	// 	new Scroll('.wrapper',{ 
			
	// 	})
	// },
	// mounted(){
	// 	this.scroll=new BScroll(document.querySelector(".wrapper"),{
			
	// 	})
	// }
  }
</script>

<style scoped>
	#category{
		/* padding-top: 44px; */
		/* padding-bottom: 400px; */
		height: 100vh;
		
	}
.content{
	 height: calc(100% - 93px);
	 overflow: hidden;
	 margin-top: 44px;
 } 
.category-nav{
	 background-color: var(--color-tint);
	 color: white;
	 position: fixed;
	 left: 0;
	 right: 0;
	 top: 0;
	 z-index: 9;
 }
</style>
