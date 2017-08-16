<template>
  <div id="app">
  	<view-box>
				<x-header slot="header" :left-options = "{showBack:false}">
					<div slot='left'>直播</div>
					<div>网易</div>
					<div slot='right'>搜索</div>
				</x-header>
				<sc :lock-y='true'>
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
					</sc>
				<scroller class='my-scroller' :on-refresh = 'refresh' :on-infinite = 'infinite' ref = 'myRef'>
					<swiper :list='swiperList' v-model = 'swiperItemIndex' :loop='true'></swiper>
					
					<marquee class="my-marquee">
						<marquee-item v-for = "list in marqueelist" :key="list.typeid">{{list.title}}</marquee-item>
					</marquee>
					
					<panel :list ='datalist'></panel>
					<panel :list ='moredatalist'></panel>
					
				</scroller>
				
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

import { ViewBox,XHeader,Tabbar, TabbarItem,Tab, TabItem,Scroller as sc,Swiper,Marquee, MarqueeItem,Panel} from 'vux'
var refreshKey = ['A','B01','B02','B03','B04','B05','B06','B07','B08','B09']
var key = 0;
var keyValue = 'A';
var start = 0;
var end = start + 9;
var initLoaded = false;

function getRefreshKey(){
	key++;
	if(key == refreshKey.length){
			key = 0;
	}
	keyValue = refreshKey[key];
}
export default {
  name: 'app',
  components:{
  	ViewBox,
  	XHeader,
  	Tabbar, TabbarItem,
  	Tab, TabItem,
  	sc,
  	Swiper,
  	Marquee, MarqueeItem,
  	Panel
  },
  created(){
  	// http://3g.163.com/touch/jsonp/sy/recommend/0-9.html
  	this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html').then( data => {
//		console.log(data);

			// 轮播图的数据
			this.swiperList = data.focus.filter(item=>{
				return item.addata ===null&& item.picInfo[0];
			}).map(item =>{
				return{
					url:item.link,
					img:item.picInfo[0].url,
					title:item.title
				}
			})
			
			//滚动新闻数据
			this.marqueelist = data.live.filter(item=>{
				return item.addata ===null&& item.picInfo[0];
			}).map(item =>{
				return{
					title:item.title,
				}
			})
			
			//首屏新闻列表的数据
			this.datalist = data.list.filter(item=>{
				return item.addata ===null&& item.picInfo[0];
			}).map(item =>{
				return{
					src:item.picInfo[0].url,
					title:item.title,
					desc:item.digest,
					url:item.link
				}
			})
			
			initLoaded = true;
			
  	})
  },
  data(){
  	
  	return{
  		swiperList:[],
			swiperItemIndex :0,
      datalist: [],
      marqueelist:[],
      moredatalist:[]
  	}
  	
  },
  methods:{
  	refresh(){
//		console.log(1);
			//下拉数据刷新
			getRefreshKey();
			this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html',{
				miss:'00',
				refresh:keyValue
			}).then( data => {
//				console.log(data)

					//刷新首屏新闻列表的数据
					this.datalist = data.list.filter(item=>{
						return item.addata ===null&& item.picInfo[0];
					}).map(item =>{
						return{
							src:item.picInfo[0].url,
							title:item.title,
							desc:item.digest,
							url:item.link
						}
					});
					
					this.$refs.myRef.finishPullToRefresh();
//					this.$vux.toast.show('hello');
//					console.log(this.datalist.length);
					this.$vux.toast.text('当前一共刷新了'+this.datalist.length+'条数据','top');
			})
  	},
  	infinite(){
//		console.log(initLoaded);
  		if (!initLoaded) {
  			this.$refs.myRef.finishInfinite();
  			return;
  		}
//		console.log(initLoaded);
//			console.log(2);
			
			// 'http://3g.163.com/touch/jsonp/sy/recommend/'+ start + '-'+ end +'.html'
			this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/'+ start + '-'+ end +'.html',{
				miss:'00',
				refresh:keyValue
			}).then(data=>{
				//下拉刷新
				console.log('http://3g.163.com/touch/jsonp/sy/recommend/'+ start + '-'+ end +'.html');
				var dataList = data.list.filter(item=>{
						return item.addata ===null && item.picInfo[0];
					}).map(item =>{
						return{
							src:item.picInfo[0].url,
							title:item.title,
							desc:item.digest,
							url:item.link
						}
					});
				this.moredatalist = this.moredatalist.concat(dataList);
				
				start +=10;
				end = start +9;
//				
				this.$refs.myRef.finishInfinite();
//				
			})
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

	.vux-header{position: absolute;width: 100%;left: 0;top: 0;}
	.tab{width: 100%;margin-top: 46px;}
	.my-marquee{margin-top: 10px;}
	.my-scroller{top: 90px;}
	.loading-layer{
		padding-bottom: 90px;
	}
	.weui-media-box_appmsg .weui-media-box__thumb{width:60px;height: 60px;}
}
</style>
