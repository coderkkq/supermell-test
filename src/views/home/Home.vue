<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
     <tab-control :titles="titles" 
                  class="tab-control"
                  @tabClick="tabClick"
                  ref="tabControl1"
                  v-show="isTabFixed"/>
    <scroll class="content"   
            ref="scroll" 
            :pull-up-load="true" 
            @pullingUp="loadMore"
            :probe-type="3"
            @scroll="contentScroll">

      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control :titles="titles" 
                   @tabClick="tabClick"
                   ref="tabControl2"/>
      <goods-list :goods="showGoods"/>
   </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
  </div>
</template>


<script>
// 页面子组件导入
import HomeSwiper from "./childComponents/HomeSwiper";
import RecommendView from "./childComponents/RecommendView";
import FeatureView from "./childComponents/FeatureView";

// 公共组件导入
import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import Scroll from "components/common/scroll/Scroll";


// 导入对home页面封装的网络请求的方法
import {getHomeMultidata, getHomeGoods} from "network/home";

//导入封装的公共方法
import {debounce} from "common/utils"
import {backTopMixin} from "common/mixin"

export default {
  name: "Home",
  components: {
    // Profile,
    HomeSwiper,
    RecommendView,
    FeatureView,

    NavBar,
    TabControl,
    GoodsList,
    Scroll,
  },
  mixins: [backTopMixin],
  data() {
    return {
      // result: null,
      banners: [],
      recommends: [],
      titles: ['流行','新款','精选'],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []}
      },
      currentType: 'pop',     //保存goods里面 数据的显示类型
      tabOffsetTop: 0,
      isTabFixed: false
    } 
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  created() {
    //1.请求多个数据
    this.getHomeMultidata()

    // 2.请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  mounted() {
    const refresh = debounce(this.$refs.scroll.refresh, 500)
      refresh()
    // console.log(this.$refs.scroll.scroll);
  },
  methods: {
    //网络请求相关：
    getHomeMultidata() {
      //1.请求多个数据
      getHomeMultidata().then(res => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
        // console.log(this.banners)
        // console.log(this.recommends)
      })
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        this.$refs.scroll.finishPullUp()
      })
    },
    loadMore() {
      this.getHomeGoods(this.currentType)
      // console.log('完成加载更多');
      this.$refs.scroll.refresh()
    },
    tabClick(index) {
      switch(index) {
        case 0: 
          this.currentType = 'pop'
          break
        case 1: 
          this.currentType = 'new'
          break
        case 2: 
          this.currentType = 'sell'
          break
      }
    },
    contentScroll(position) {
      this.isShowBackTop = (-position.y) > 1000
      // console.log(this.$refs.tabControl2.$el.offsetTop); 这个是不准确的
      this.isTabFixed = (-position.y) >  this.tabOffsetTop
    },
    swiperImageLoad() {
      // console.log(this.$refs.tabControl2.$el.offsetTop)
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    }
  }
}
</script>

<style scoped>
  #home {
    position: relative;
    /*padding-top: 44px;*/
    /*vh：视口，100vh：100个视口的高度*/
    height: 100vh;
  }
 .home-nav {
   background-color: var(--color-tint);
   color: #ffffff;
   /*position: fixed;*/
   /*left: 0;*/
   /*right: 0;*/
   /*top: 0;*/
   /*z-index: 9;*/
 }
 /*轮播图内容*/
 .content {
   position: absolute;
   top: 44px;
   bottom: 49px;
   left: 0;
   right: 0;
   overflow: hidden;
 }
 .tab-control {
   position: relative;
   z-index: 9;
   background-color: #fff;
 }
</style>
