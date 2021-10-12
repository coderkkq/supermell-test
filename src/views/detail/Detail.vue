<template>
  <div id="detail">
    <!-- <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"/> -->
   <!-- <scroll class="content" ref="scroll" @scroll="contentScroll" :probe-type="3">
     <detail-swiper :top-images="topImages"></detail-swiper>
     <detail-base-info :goods="goods"/>
     <detail-shop-info :shop="shop"/>
     <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
     <detail-param-info ref="params" :param-info="paramInfo"/>
     <detail-comment-info ref="comment" :comment-info="commentInfo"/>
     <goods-list ref="recommend" :goods="recommends"/>
   </scroll>
    <detail-bottom-bar @addToCart="addCart"/>

    <back-top @click.native="backClick" v-show="isShowBackTop"/> -->
  </div>
</template>

<script>
// 导入页面子组件
// import DetailNavBar from './childComponents/DetailNavBar'

// 导入公共子组件

// 导入请求方法
import {getDetail} from 'network/detail'

export default {
 name: "Detail",
 components: {
  //  DetailNavBar
 },
 data() {
   return {
     iid: null,
     topImages: [],
     goods: {}
   }
 },
 created() {
   this.iid = this.$route.query.iid
   console.log($route.query.iid);

   getDetail(this.iid).then(res => {
     const data = res.result;
     this.topImages = data.itemInfo.topImages;
     this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
   })

 }

}
</script>

<style scoped>
   #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: calc(100vh - 44px - 49px);
  }
  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
  /* .content {
    height: 100%;
  } */
</style>