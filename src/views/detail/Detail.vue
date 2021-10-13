<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"/>
    <detail-swiper :top-images="topImages"/>
    <detail-base-info :goods="goods"/>
    <detail-shop-info/>
    <detail-goods-info :detail-info="detailInfo"/>
    <detail-param-info :param-info="paramInfo"/>
    <detail-comment-info :comment-info="commentInfo"/>

     <detail-bottom-bar />
    <ul>
      <li>列表1</li>
      <li>列表2</li>
      <li>列表3</li>
      <li>列表4</li>
      <li>列表5</li>
      <li>列表6</li>
      <li>列表7</li>
      <li>列表8</li>
      <li>列表9</li>
      <li>列表10</li>
      <li>列表11</li>
      <li>列表12</li>
      <li>列表13</li>
      <li>列表14</li>
      <li>列表15</li>
      <li>列表16</li>
      <li>列表17</li>
      <li>列表18</li>
      <li>列表19</li>
      <li>列表20</li>
      <li>列表21</li>
      <li>列表22</li>
      <li>列表23</li>
      <li>列表24</li>
      <li>列表25</li>
      <li>列表26</li>
      <li>列表27</li>
      <li>列表28</li>
      <li>列表29</li>
      <li>列表30</li>
    </ul>
  </div>
</template>

<script>
import DetailNavBar from "./childComponents/DetailNavBar"
import DetailSwiper from "./childComponents/DetailSwiper"
import DetailBaseInfo from "./childComponents/DetailBaseInfo"
import DetailShopInfo from "./childComponents/DetailShopInfo"
import DetailGoodsInfo from "./childComponents/DetailGoodsInfo"
import DetailParamInfo from "./childComponents/DetailParamInfo"
import DetailCommentInfo from "./childComponents/DetailCommentInfo"
import DetailBottomBar from "./childComponents/DetailBottomBar"

// 请求方法
import {getDetail, Goods, Shop, GoodsParam, getRecommend} from "network/detail"

export default {
 name: "Detail",
 components: {
   DetailNavBar,
   DetailSwiper,
   DetailBaseInfo,
   DetailShopInfo,
   DetailGoodsInfo,
   DetailParamInfo,
   DetailCommentInfo,
   DetailBottomBar
 },
 data() {
   return {
     iid: null,
     topImages: [],
     goods: {},
     shop: {},
     detailInfo: {},
     paramInfo: {},
     commentInfo: {}
   }
 },
 created() {
   this.iid = this.$route.params.iid
   getDetail(this.iid).then(res => {
     const data = res.result;
     this.topImages = data.itemInfo.topImages
     this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
     this.shop = new Shop(data.shopInfo)
     this.detailInfo = data.detailInfo
     this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

       if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }
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
</style>