<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import Header from 'components/header/header';
  import Goods from 'components/goods/goods';
  import Seller from 'components/seller/seller';
  import Ratings from 'components/ratings/ratings';
  import VueRouter from 'vue-router';
  import {urlParse} from 'common/js/util';

  const ERR_OK = 0;

  export default {
    name: 'app',
    data () {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id || null;
          })()
        }
      };
    },
    created () {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        var body = response.body;
        if (body.errno === ERR_OK) {
          // this.seller = body.data;
          this.seller = Object.assign({}, this.seller, body.data);
        }
      });
    },
    components: {
      'v-header': Header
    },
    router: new VueRouter({
      linkActiveClass: 'active',
      routes: [
        {path: '/goods', component: Goods},
        {path: '/seller', component: Seller},
        {path: '/ratings', component: Ratings}
      ]
    })
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/mixin.styl"

  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)

</style>
