<template>
  <div id="modal">

    <transition v-if="isShowModal === true" name="modal">
      <div class="modal-mask" v-on:click="clickSubMenu">
        <div class="modal-wrapper">
          <div class="modal-container">
            <div class="modal-body">
              <slot name="body">
              </slot>
              <div v-if="showModalType === 'target'" class="sub-nav" slot="body">
                <ul>
                  <router-link to="/create-targetlist">
                    <li class="create">新規作成</li>
                  </router-link>
                  <router-link to="/targetlist">
                    <li class="list">リスト</li>
                  </router-link>
                </ul>
              </div>
              <div v-else-if="showModalType === 'flame'" class="sub-nav" slot="body">
                <ul>
                  <router-link to="/create-ng-timeframe">
                    <li class="create">新規作成</li>
                  </router-link>
                  <router-link to="/ng-timeframe">
                    <li class="list">リスト</li>
                  </router-link>
                </ul>
              </div>
              <div v-else-if="showModalType === 'reachmax'" class="sub-nav" slot="body">
                <ul>
                  <router-link to="/create-reachmax">
                    <li class="create">新規作成</li>
                  </router-link>
                  <router-link to="/reachmax">
                    <li class="list">リスト</li>
                  </router-link>
                </ul>
              </div>
              <div v-else-if="showModalType === 'setting'" class="sub-nav" slot="body">
                <ul>
                  <router-link :to="{name: 'PasswordChange'}">
                    <li class="pass">パスワード変更</li>
                  </router-link>
                  <!--<router-link to="/#">
                    <li class="help">ヘルプ</li>
                  </router-link>-->
                </ul>
              </div>

            </div>
          </div>
        </div>
      </div>
    </transition>

    <nav id="main-nav">
      <ul>
        <li v-on:click="clickHomeNav" v-bind:class='{"is-active": activeNavType === "home"}'>
          <a v-on:click="clickHomeNav" v-bind:class='{"active": activeNavType === "home"}'>
            ホーム
          </a>
        </li>
        <li v-on:click="clickTargetNav" v-bind:class='{"is-active": activeNavType === "target"}'>
          <a v-on:click="clickTargetNav" v-bind:class='{"active": activeNavType === "target"}'>
            対象者
          </a>
        </li>
        <li v-on:click="clickFlameNav" v-bind:class='{"is-active": activeNavType === "flame"}'>
          <a v-on:click="clickFlameNav" v-bind:class='{"active": activeNavType === "flame"}'>
            枠制限
          </a>
        </li>
        <li v-on:click="clickReachMaxNav" v-bind:class='{"is-active": activeNavType === "reachmax"}'>
          <a v-on:click="clickReachMaxNav" v-bind:class='{"active": activeNavType === "reachmax"}'>
            リーチマックス
          </a>
        </li>
        <li v-on:click="clickSettingNav" v-bind:class='{"is-active": activeNavType === "setting"}'>
          <a v-on:click="clickSettingNav" v-bind:class='{"active": activeNavType === "setting"}'>
            設定
          </a>
        </li>
        <!--<li v-on:click="clickHelpNav" v-bind:class='{"is-active": activeNavType === "help"}'>
          <a v-on:click="clickHelpNav" v-bind:class='{"active": activeNavType === "help"}'>
            ヘルプ
          </a>
        </li>-->
      </ul>
    </nav>

  </div>
</template>

<script>

  const axios = require('axios');
  export default {
    name: 'Sidebar',
    data() {
      return {
        routes: this.$router.options.routes[0].children,
        fetchedData: '',
        activeNavType: '',
        isShowModal: false,
        showModalType: '',
      }
    },

    created() {
      this.fetchData();
    },

    methods: {
      fetchData: function () {
        axios({
          method: 'GET',
          headers: {
            Authorization: `Bearer ${this.jwt}`
          },
          url: `${this.apiBaseUrl}/reachmax/`
        })
          .then(response => (this.fetchedData = response))
          .catch(error => console.log(error));
      },

      clickHomeNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'home';
        this.showModalType = '';
        this.clickSubMenu();
        this.$router.push({name: 'TOP'});
      },

      clickTargetNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'target';
        this.showModalType = 'target';
      },

      clickFlameNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'flame';
        this.showModalType = 'flame';
      },

      clickReachMaxNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'reachmax';
        this.showModalType = 'reachmax';
      },

      clickSettingNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'setting';
        this.showModalType = 'setting';
      },
      clickHelpNav: function() {
        this.isShowModal = true;
        this.activeNavType = 'help';
        this.showModalType = '';
        this.clickSubMenu();
        this.$router.push({name: 'TOP'});
      },

      clickSubMenu: function() {
        this.isShowModal = false;
        this.activeNavType = '';
      },
    },

    computed: {
      jwt() {
        return this.$store.state.jwt
      },
      apiBaseUrl() {
        return this.$store.state.apiBaseUrl
      },
      tableauBaseUrl() {
        return this.$store.state.tableauBaseUrl
      },
      tableauLink() {
        let lastData = this.fetchedData.data.length - 1;
        let lastReachmaxId = this.fetchedData.data[lastData].id;
        return this.tableauBaseUrl + lastReachmaxId;
      }
    }
  }
</script>

<style scoped>
/* http://meyerweb.com/eric/tools/css/reset/
     v2.0 | 20110126
     License: none (public domain)
  */
  /*
    /* HTML5 display-role reset for older browsers 
  article, aside, details, figcaption, figure,
  footer, header, hgroup, menu, nav, section {
    display: block;
  }*/
  html, body, div, span, applet, object, iframe,
  h1, h2, h3, h4, h5, h6, p, blockquote, pre,
  a, abbr, acronym, address, big, cite, code,
  del, dfn, em, img, ins, kbd, q, s, samp,
  small, strike, strong, sub, sup, tt, var,
  b, u, i, center,
  dl, dt, dd, ol, ul, li,
  fieldset, form, label, legend,
  table, caption, tbody, tfoot, thead, tr, th, td,
  article, aside, canvas, details, embed,
  figure, figcaption, footer, header, hgroup,
  menu, nav, output, ruby, section, summary,
  time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
  }

  ol, ul li {
    list-style: none;
  }
  #main-nav a {
    /*text-decoration: none;*/
  }
  #main-nav .active {
    color: #fff;
    text-decoration: none;
  }
  #main-nav .active{
    background-color: #31AEED;
  }
  

  #main-nav {
    width: 110px;
    top: 55px;
    left: 0;
    height: 100%;
    position: fixed;
    overflow: hidden;
    z-index: 4;
    box-shadow: 1px 0px 10px rgba(51, 51, 51, 0.3);
    background: #fff;
  }
  #main-nav li{
    position: relative;
    border-bottom: 1px solid #AFAFAF;
  }
  #main-nav a{
    cursor: pointer;
    padding: 70px 0 20px 0;
    font-size: 14px;
    font-weight: bold;
    text-align: center;
    display: block;
    color: #31AEED;
  }
  #main-nav li:hover {
    background: #D1E8F4;
  }
  #main-nav li::before{
    position: absolute;
    background-image: url('img/home.png');
    background-repeat: no-repeat;
    background-size: contain;
    content: "";
    display: block;
    width: 40px;
    height: 40px;
    top: 24px;
    left: 50%;
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
  #main-nav li:nth-child(2)::before{
    background-image: url("img/target.png");
    width: 38px;
    height: 40px;
  }
  #main-nav li:nth-child(3)::before{
    background-image: url("img/flame.png");
    width: 36px;
    height: 40px;
  }
  #main-nav li:nth-child(4)::before{
    background-image: url("img/reachmax.png");
    width: 44px;
    height: 40px;
  }
  #main-nav li:nth-child(5)::before{
    background-image: url("img/setting.png");
    width: 36px;
    height: 40px;
  }
  #main-nav li:nth-child(6)::before{
    background-image: url("img/help.png");
    width: 37px;
    height: 37px;
  }

  #main-nav li.is-active:before {
    position: absolute;
    background-image: url('img/home02.png');
    background-size: contain;
    content: "";
    display: block;
    width: 40px;
    height: 40px;
    top: 24px;
    left: 50%;
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
  #main-nav li.is-active:nth-child(2):before {
    background-image: url('img/target02.png');
    width: 38px;
    height: 40px;
  }
  #main-nav li.is-active:nth-child(3):before {
    background-image: url('img/flame02.png');
    width: 36px;
    height: 40px;
  }
  #main-nav li.is-active:nth-child(4):before {
    background-image: url('img/reachmax02.png');
    width: 44px;
    height: 40px;
  }
  #main-nav li.is-active:nth-child(5):before {
    background-image: url('img/setting02.png');
    width: 36px;
    height: 40px;
  }
  #main-nav li.is-active:nth-child(6):before {
    background-image: url('img/help02.png');
    width: 36px;
    height: 40px;
  }


  .sub-nav li{
    position: relative;
    border-bottom: 1px solid #AFAFAF;
    text-align: left;
    padding: 17px 0 17px 40px;
    color: #31AEED;
    display: block;
    font-size: 14px;
  }
  .sub-nav li::after{
    content: '';
    width: 6px;
    height: 6px;
    border: 0px;
    border-top: solid 1px #31AEED;
    border-right: solid 1px #31AEED;
    -ms-transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
    position: absolute;
    top: 50%;
    right: 25px;
    margin-top: -4px;
  }

  .sub-nav a{
    text-decoration: none;
  }
  .sub-nav li:hover{
    background: #D1E8F4;
  }

  .sub-nav .create::before {
    background: url('img/create.png') no-repeat;
    background-size: contain;
    content: "";
    display: block;
    width: 20px;
    height: 20px;
    position: absolute;
    top: 35%;
    left: 10px;
  }
  .sub-nav .list::before {
    background: url('img/list.png') no-repeat;
    background-size: contain;
    content: "";
    display: block;
    width: 20px;
    height: 20px;
    position: absolute;
    top: 35%;
    left: 10px;
  }
  .sub-nav .pass::before {
    background: url('img/pass.png') no-repeat;
    background-size: contain;
    content: "";
    display: block;
    width: 20px;
    height: 23px;
    position: absolute;
    top: 27%;
    left: 12px;
  }
  /*.sub-nav .help::before {
    background: url('img/help.png') no-repeat;
    background-size: contain;
    content: "";
    display: block;
    width: 20px;
    height: 23px;
    position: absolute;
    top: 26%;
    left: 10px;
  }*/

  #sub-nav .sub-menu-nav {
    position: absolute;
    top: 0;
    width: 200px;
    height: 100%;
    color: #31AEED;
    background: #fff;
    transform: translate(-110px, 0);
    z-index: 2;
    transition-duration: .15s;
  }
  #sub-nav .sub-menu-nav.is-active {
    transform: translate(110px, 0);
  }

  .modal-body {
    top: 55px;
  }
  .modal-mask {
    position: fixed;
    z-index: 3;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    transition: opacity .2s ease;
  }
  .modal-wrapper {
    width: 100%;
    height: 100%;
  }
  .modal-container {
    position: absolute;
    top: 0;
    width: 200px;
    height: 100%;
    color: #31AEED;
    background: #fff;
    text-align: left;
    transform: translate(110px, 0);
    z-index: 2;
    box-shadow: 1px 0px 10px rgba(51, 51, 51, 0.3);
  }
  .modal-enter {
    opacity: 0;
  }
  .modal-leave-active {
    opacity: 0;
  }

  @media screen and (max-width: 767px) {
    #main-nav {
      display: none;
    }
  }

</style>
