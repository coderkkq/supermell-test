<template>
  <div class="goods-list">
    <div class="goods-list-item" v-for="item in goods">
      <img :src="item.show.img" alt="">
      <div class="good-info">
        <p>{{item.title}}</p>
        <span class="price">{{item.price}}</span>
        <span class="collect">{{item.cfav}}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "GoodsList",
  props: {
    goods: {
      type: Array,
      default() {
        return []
      }
    },
  }
}
</script>

<style scoped>
  .goods-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    padding: 2px;
  }
  .goods-list-item {
    position: relative;
    width: 48%;
    padding-bottom: 40px;
  }
  .goods-list-item img {
    width: 100%;
    border-radius: 5px;
  }
  .good-info {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 5px;
    text-align: center;
    font-size: 12px;
  }
  .good-info p {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    margin-bottom: 3px;
  }
  .good-info .price {
    margin-right: 20px;
    color: var(--color-high-text);
  }
  .good-info .collect {
    position: relative;
  }
  .good-info .collect::before {
    position: absolute;
    top: -1px;
    left: -15px;
    content: '';
    width: 14px;
    height: 14px;
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }
</style>
