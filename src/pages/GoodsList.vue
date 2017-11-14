<template>
  <div>
    <nav-header></nav-header>
    <bread-crump>
      <span slot="breadCrump">Goods</span>
    </bread-crump>
    <div class="accessory-result-page accessory-page">
      <div class="container">
        <div class="filter-nav">
          <span class="sortby">Sort by:</span>
          <a href="javascript:void(0)" @click="filterByWay('default')" class="default" :class="{'cur':filterBy=='default'}">Default</a>
          <a href="javascript:void(0)" class="price" :class="{'cur':filterBy=='price'}" @click="filterByWay('price')">Price
            <svg class="icon icon-arrow-short">
              <use xlink:href="#icon-arrow-short"></use>
            </svg>
          </a>
          <a href="javascript:void(0)" class="filterby stopPop">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter" v-if="filterBy=='price'">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd><a :class="{'cur': selectedPriceIdx==-1}" href="javascript:void(0)">All</a></dd>
              <dd v-for="(filter, idx) in priceFilter" :key="idx">
                <a href="javascript:void(0)" :class="{'cur': selectedPriceIdx==idx}" @click="priceFilterClick(idx)">{{filter.startPrice}} - {{filter.endPrice}}</a>
              </dd>
            </dl>
          </div>

          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="(item, index) in goodsList" :key="index">
                  <div class="pic">
                    <a href="#"><img :src="`./../../static/${item.prodcutImg}`" alt=""></a>
                  </div>
                  <div class="main">
                    <div class="name">{{item.productName}}</div>
                    <div class="price">{{item.prodcutPrice}}</div>
                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
    <general-footer></general-footer>
  </div>
</template>
<script>
import axios from 'axios'
import NavHeader from '@/components/Header'
import generalFooter from '@/components/Footer'
import BreadCrump from '@/components/BreadCrump'
export default{
  components: {
    NavHeader: NavHeader,
    generalFooter: generalFooter,
    BreadCrump: BreadCrump
  },
  data () {
    return {
      selectedPriceIdx: -1,
      goodsList: [],
      filterBy: 'default',
      priceFilter: [
        {
          startPrice: '0.00',
          endPrice: '500.00'
        },
        {
          startPrice: '500.00',
          endPrice: '1000.00'
        },
        {
          startPrice: '1000.00',
          endPrice: '2000.00'
        }
      ]
    }
  },
  mounted () {
    this.getGoodsList()
  },
  methods: {
    getGoodsList () {
      axios.get('/goods').then(res => {
        this.goodsList = res.data.result
      })
    },
    priceFilterClick (idx) {
      this.selectedPriceIdx = idx
    },
    filterByWay (way) {
      this.filterBy = way
    }
  }
}
</script>


<style lang="less" scoped>
@import "./../assets/style/product.css";
@import 'http://at.alicdn.com/t/font_442866_t7x3gdu8ooyo80k9.css';
</style>
