<template>
  <div class="container" id="app">
    <div class="tags-panel">
      <ul class="tags-lines">
        <li class="tags-line" data-type="brand">
          <div class="tags-title">品牌：</div>
          <ul class="tags">
            <li v-for="(brand, index) in brand_list" v-if="index < 10" :key="index">
              <button v-on:click="changeLimit(brand.brand_name, cinemaDis)" target="_self">{{brand.brand_name}}</button>
            </li>
          </ul>
        </li>
        <li class="tags-line tags-line-border" data-type="district">
          <div class="tags-title">行政区：</div>
          <ul class="tags">
            <li v-for="(position, index) in position_list" v-if="index < 12" :key="position.position_name">
              <button v-on:click="changeLimit(cinemaBrand, position.position_name)" target="_self">{{position.position_name}}</button>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="cinemas-list">
      <h2 class="cinemas-list-header">影院列表</h2>
      <div v-for="(theatre, index) in cinema_list" v-if="index < 10" class="cinema-cell" :key="theatre.cinema_name">
        <!--<div class="cinema-cell" v-for="item in theatredata" :key="item.id">-->
        <div class="cinema-info">
          <a  class="cinema-name" :href="theatre.href" target="_blank" :title="theatre.cinema_name">{{theatre.cinema_name}}</a>
          <p class="cinema-address">{{theatre.address}}</p>
        </div>
        <div class="buy-btn">
          <a :href="'/nuxt/' + href + '?filmName=' + filmName + '&cinemaName=' + theatre.cinema_name" target="_blank">选座购票</a>
        </div>
        <div class="price">
          <span class="rmb red">￥</span>
          <span class="price-num red">
                <span class="stonefont">36</span>
            </span>
          <span> 起</span>
        </div>
      </div>

    </div>
    <!--分页栏--每页10项，共50页-->
    <div class="pag-nav">
      <b-pagination align="center" :total-rows="500" v-model="currentPage" :per-page="10" @click.native="changeLimit(cinemaBrand, cinemaDis)"></b-pagination>
    </div>
  </div>
</template>

<script scoped>
  import axios from '~/plugins/axios'
  export default {
    head() {
      return {'title': '影院列表'}
    },
    data () {
      return {
        filmName: "",   // 从电影详情页面跳转过来选影院
        currentPage: 1,
        numEachPage: 10,
        cinemaBrand: '全部',
        cinemaDis: '全部',
        cinema_list: [],
        href: '/cinema-detail-page',    // 转到影院详情页面
        brand_list: [
          {brand_name: "全部"},
          {brand_name: "飞扬影城"},
          {brand_name: "大地影院"},
          {brand_name: "万达影城"},
          {brand_name: "星美国际影城"},
          {brand_name: "横店电影城"},
          {brand_name: "中影国际影城"},
          {brand_name: "保利国际影城"},
          {brand_name: "CGV影城"}
        ],
        position_list: [
          {position_name: "全部"},
          {position_name: "白云区"},
          {position_name: "海珠区"},
          {position_name: "番禺区"},
          {position_name: "天河区"},
          {position_name: "越秀区"},
          {position_name: "荔湾区"},
          {position_name: "花都区"},
          {position_name: "黄埔区"},
          {position_name: "南沙区"},
          {position_name: "增城市"},
          {position_name: "从化市"}
        ],
      }
    },
    async created(){
      await this.getCinemaAll()
    },
    async asyncData({context, route}) {
      let choosedFilmName = route.query.filmName == null ? "" : route.query.filmName;
      console.log(choosedFilmName)
      return {filmName: choosedFilmName}
    },
    methods: {
      async getCinemaAll(){
        console.log()
        try {
          // console.log(currentPage)
          let {data} = await axios.get('/api/getCinemaByDistrict', {params: {currentPage: 1, district: '全部'}})
          this.cinema_list = data.data
        } catch (e) {
          // console.log(e)
        }
      },
      async changeLimit(brandName, distName) {
        this.cinemaBrand = brandName;
        this.cinemaDis = distName;
        let nowcinema = [];
        try {
          if(this.cinemaBrand=="全部"){
            let {data} = await axios.get('/api/getCinemaByDistrict', {params: {district: this.cinemaDis, currentPage: this.currentPage}})
            if (!data.errorCode) {
              nowcinema = data.data
            }
          }
          else if(this.cinemaDis=="全部"){
            let {data} = await axios.get('/api/getCinemaByBrand', {params: {brand: this.cinemaBrand, currentPage: this.currentPage}})
            if (!data.errorCode) {
              nowcinema = data.data
            }
          }
          else{
            let {data} = await axios.get('/api/getCinemaByAll', {params: {brand: this.cinemaBrand, district: this.cinemaDis, currentPage: this.currentPage}})
            if (!data.errorCode) {
              nowcinema = data.data
            }
          }
        } catch (e) {
          console.log(e)
        }
        this.cinema_list = nowcinema
      }
    },
  }
</script>

<style>
  .container {
    min-width: max-content;
  }
  div{
    display: block;
  }
  /*body {*/
    /*background: none;*/
    /*background-size: cover;*/
  /*}*/
  li{
    display: list-item;
    text-align: -webkit-match-parent;
  }
  a{
    text-decoration: none;
    background-color: transparent;
  }
  p{
    margin: 0;
  }
  button{
    background: transparent;
    border: none;
    color: #dbe1ec;
    padding: 0.5px;
    margin: 0;
    cursor: pointer;
  }
  /*button .active{*/
    /*background: #721c24;*/
  /*}*/
  .container{
    width: 1200px;
    margin-top: 100px;
  }
  .tags-panel{
    border: 1px solid #e5e5e5;
    padding: 0 20px;
    margin: 40px 0;
    position: relative;
  }
  .tags-panel li, .tags-panel ul{
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  .tags-line{
    padding: 10px 0!important;
  }
  .tags-title{
    height: 24px;
    line-height: 24px;
    float: left;
    color: #999;
    font-size: 14px;
  }
  .tags-panel ul.tags{
    margin-left: 40px;
  }
  .tags li{
    border-radius: 14px;
    padding: 3px 9px;
    display: inline-block;
    margin-left: 12px;
  }
  .tags li.active{
    background: #dbe1ec;
    color: #47494e;
  }
  .tags li a{
    color: #dbe1ec;
    font-size: 14px;
  }
  .tags li.active a{
    color: #47494e;
  }
  .tags-line-border{
    border-top: 1px dashed #e5e5e5;
  }
  .cinemas-list .cinemas-list-header{
    font-size: 18px;
    color: #7f828b;
    border-left: 4px solid #e4606d;
    padding-left: 6px;
    line-height: 18px;
    margin: 0;
  }
  h2{
    display: block;
    font-size: 1.5em;
    -webkit-margin-before: 0.83em;
    -webkit-margin-after: 0.83em;
    -webkit-margin-start: 0;
    -webkit-margin-end: 0;
    font-weight: bold;
  }
  .cinemas-list .cinema-cell{
    display: block;
    padding: 20px 0;
    border-bottom: 1px dashed #e5e5e5;
  }
  .cinemas-list .cinema-info{
    display: inline-block;
    max-width: 80%;
  }
  .cinemas-list .cinema-name{
    display: inline-block;
    font-size: 16px;
    line-height: 18px;
    color: #eeeeee!important;
    margin-bottom: 10px;
  }
  .cinemas-list .cinema-address{
    font-size: 14px;
    line-height: 14px;
    color: #999;
  }
  .cinemas-list .buy-btn{
    float: right;
    width: 80px;
    height: 45px;
    line-height: 45px;
    margin-left: 36px;
    cursor: pointer;
  }
  .cinemas-list .buy-btn a{
    display: inline-block;
    width: 100%;
    height: 30px;
    color: #fff;
    background-color: #e4606d;
    font-size: 14px;
    line-height: 30px;
    border-radius: 100px;
    text-align: center;
    -webkit-box-shadow: 0 2px 10px -2px #e4606d;
    box-shadow: 0 2px 10px -2px #e4606d;
  }
  .cinemas-list .price{
    float: right;
    font-size: 12px;
    color: #999999;
    height: 45px;
    line-height: 45px;
  }
  .cinemas-list .price .rmb{
    margin-right: -4px;
  }
  .red{
    color: #e4606d;
  }
  .cinemas-list .price .price-num{
    font-size: 16px;
    margin-right: -3px;
    /*margin-left: 1px;*/
    font-weight: 700;
  }
  .cinema-pager{
    margin-top: 15px;
    text-align: center;
  }
  .list-pager{
    list-style: none;
  }
  .list-pager li.active{
    border-color: #e4606d;
    background-color: #e4606d;
  }
  .list-pager li{
    display: inline-block;
    height: 32px;
    margin: 0 4px;
    border: 1px solid #d8d8d8;
    line-height: 30px;
    text-align: center;
    color: #999999;
    background-color: #dbe1ec;
  }
  .list-pager a{
    display: block;
    padding: 0 12px;
    color: #333333;
  }
  .list-pager a{
    font-size: 14px;
  }
  .list-pager li.sep{
    padding: 0 12px;
  }
  .pag-nav {
    margin-top: 20px;
    margin-bottom: 15px;
    /*position: relative;*/
    bottom: -20px;
  }
</style>


