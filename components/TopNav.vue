<template>
  <b-navbar toggleable="lg"  class="top-navbar" style="background-color:#2E294E;" type="dark">
    <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
    <b-navbar-brand href="#">STAR MOVIE</b-navbar-brand>
    <b-collapse is-nav id="nav_collapse">
      <!--导航栏-->
      <b-navbar-nav>
        <b-nav-item-dropdown class="city-name" text="广州" left open>
          <b-dropdown-item href="#">定位城市：<span class="city-name">广州</span></b-dropdown-item>
          <b-dropdown-item href="#"><b-list-group class="city-list"></b-list-group></b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item href="/nuxt">首页</b-nav-item>
        <b-nav-item href="/nuxt/cinema">影院</b-nav-item>
        <b-nav-item-dropdown text="电影" left no-caret>
          <b-dropdown-item href="/nuxt/movies?sort_ID=1">正在热映</b-dropdown-item>
          <b-dropdown-item href="/nuxt/movies?sort_ID=2">即将上映</b-dropdown-item>
          <b-dropdown-item href="/nuxt/movies?sort_ID=3">经典影片</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item-dropdown text="资讯" left no-caret>
          <b-dropdown-item href="/nuxt/hot-news">热点资讯</b-dropdown-item>
          <b-dropdown-item href="/nuxt/all-news">全部资讯</b-dropdown-item>
          <b-dropdown-item href="/nuxt/all-videos">全部视频</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input v-model="inputText" type="text" size="sm" placeholder="Search"/>
          <b-button :href="'/nuxt/movies?sort_ID='+inputText" size="sm" class="my-sm-0" type="submit">Search</b-button>
        </b-nav-form>

        <b-nav-item-dropdown right>
          <!-- Using button-content slot -->
          <template slot="button-content">
            <img  v-if="$store.state.authUser" class="portrait rounded-circle" src="../assets/img/logo/login-default-portrait.png">
            <img  class="portrait rounded-circle" src="../assets/img/logo/logout-default-portrait.png" v-else>
          </template>
          <b-dropdown-item v-if="$store.state.authUser"><nuxt-link to="/users">个人主页</nuxt-link></b-dropdown-item>
          <b-dropdown-item v-if="$store.state.authUser" v-on:click="logout()">退出</b-dropdown-item>
          <b-dropdown-item v-else><nuxt-link to="/login">登录</nuxt-link>/<nuxt-link to="/register">注册</nuxt-link></b-dropdown-item>
        </b-nav-item-dropdown>

      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
  export default {
    name: 'top-nav',
    data(){
      return {
        inputText: ''
      }
    },
    methods: {
      async logout() {
        await this.$store.dispatch('logout')
        // this.$router.replace("/login")
      }
    }
  }
</script>

<style scoped>
  .portrait {
    width: 40px;
    height: 40px;
    background-color: white;
  }

  .top-navbar {
    color: white;
    width: 100%;
    background-color: #282c37;
    position: fixed;
    font-size: 18px;
    top: 0;
    z-index: 99;
  }
  @media screen and (min-width: 960px) {
    .top-navbar {
      height: 60px;
    }
  }
  @media screen and (max-width: 959px) {
    .top-navbar {
      height: fit-content;
    }
  }
</style>

