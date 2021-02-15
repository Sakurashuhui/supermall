<template>
	<div class="wrapper" ref="wrapper" >
		<div class="content">
		<slot></slot>
		</div>
	</div>
</template>

<script>
		import BScroll from 'better-scroll';
	export default{
		name:"Scroll",
		props:{
			probeType:{
				type:Number,
				default:0
			},
			pullUpLoad:{
				type:Boolean,
				default:false
			}
		    
		},
		data(){
			return {
				scroll:null
			}
		},
		mounted(){
			//1.创建Bscroll对象
		   this.scroll=new BScroll(this.$refs.wrapper,{
			   observeDom:true,
			   click:true,
			   mouseWheel:true,
			   probeType:this.probeType,
			   pullUpLoad:this.pullUpLoad
		   })
		   //2.监听滚动的区域
		   this.scroll.on('scroll',(position)=>{
			   //console.log(position);
			   this.$emit('scroll',position);
		   })
		   this.scroll.on('pullingUp',()=>{
			   // console.log("上拉加载更多");
			   this.$emit('pullingUp');
		   })
		   
		},
		methods:{
			scrollTo(x,y,time=300){
			this.scroll.scrollTo(x,y,time)
			},
			refresh(){
				this.scroll.refresh()
			}
			
		}
		
	}
</script>

<style>
</style>
