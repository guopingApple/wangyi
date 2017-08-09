<template>
  <div id="app">
  	<view-box>
				<x-header slot="header" :left-options = "{showBack:false}">
					<div slot='left'>直播</div>
					<div>网易</div>
					<div slot='right'>搜索</div>
				</x-header>
				
				<scroller :lock-y='true'>
					<div class="tab">
						<tab>
								<tab-item selected>推荐</tab-item>
								<tab-item>要闻</tab-item>
								<tab-item>游戏</tab-item>
								<tab-item>科技</tab-item>
								<tab-item>体育</tab-item>
								<tab-item>互联网</tab-item>
						</tab>
					</div>
				</scroller>
				
				<swiper :list='swiperList' v-model = 'swiperItemIndex' :loop='true'>
						
				</swiper>
				
				<marquee class="my-marquee">
					<marquee-item>1111</marquee-item>
					<marquee-item>2222</marquee-item>
					<marquee-item>333</marquee-item>
					<marquee-item>444</marquee-item>
				</marquee>
				
				<panel :list ='datalist'>
					
				</panel>
				
				<tabbar slot = 'bottom'>
					<tabbar-item>
							<img src="./assets/icon_nav_button.png" slot='icon'/>
							<span slot='label'>首页</span>
					</tabbar-item>
					<tabbar-item>
							<img src="./assets/icon_nav_article.png" slot='icon'/>
							<span slot='label'>推荐</span>
					</tabbar-item>
					<tabbar-item>
							<img src="./assets/icon_nav_cell.png" slot='icon'/>
							<span slot='label'>我的</span>
					</tabbar-item>
				</tabbar>
  	
  	</view-box>
    <router-view></router-view>
  </div>
</template>

<script>

import { ViewBox,XHeader,Tabbar, TabbarItem,Tab, TabItem,Scroller,Swiper,Marquee, MarqueeItem,Panel} from 'vux'

export default {
  name: 'app',
  components:{
  	ViewBox,
  	XHeader,
  	Tabbar, TabbarItem,
  	Tab, TabItem,
  	Scroller,
  	Swiper,
  	Marquee, MarqueeItem,
  	Panel
  },
  data(){
  	var datalist = [];
  	for (var i = 0;i<10;i++) {
  		datalist.push({
  			src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题一',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: '/component/cell'
  		})
  	}
  	return{
  		swiperList:[{
			  url: 'javascript:',
			  img: 'https://static.vux.li/demo/1.jpg',
			  title: '送你一朵fua'
			}, {
			  url: 'javascript:',
			  img: 'https://static.vux.li/demo/2.jpg',
			  title: '送你一辆车'
			}, {
			  url: 'javascript:',
			  img: 'https://static.vux.li/demo/3.jpg',
			  title: '送你一次旅行'
			}],
			swiperItemIndex :0,
			type: '1',
      datalist: datalist
  	}
  }
}
</script>

<style lang="less">
	@import '~vux/src/styles/reset.less';
	html,body{
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
}
.tab{width: 600px;}
</style>
