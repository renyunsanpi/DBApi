<template>
  <div class="head">
    <div class="logo">DBApi</div>
    <div class="menus">
      <div class="menu iconfont icon-database" @click="clickMenu('/datasource')">
        {{ $t('m.datasource') }}
      </div>
      <div class="menu iconfont icon-api" @click="clickMenu('/api')">
        API
      </div>
      <div class="menu iconfont icon-quanxian" @click="clickMenu('/token')">
        {{ $t('m.authority') }}
      </div>
      <div class="menu iconfont icon-shezhi">
        {{ $t('m.settings') }}
        <div class="submenus">
          <div class="submenu" @click="clickMenu('/setting/password')">{{ $t('m.change_pass') }}</div>
          <div class="submenu" @click="clickMenu('/setting/firewall')">{{ $t('m.firewall') }}</div>
        </div>
      </div>

    </div>
    <div class="right">
      <div class="mode">{{ this.$store.state.mode }}</div>
      <!--      <span>{{ languageName }}</span>
            <span class="lang el-icon-arrow-down">
            </span>
            <div class="options">
              <div class="option" v-for="item in langs" @click="changeLanguage(item)">{{ item.name }}</div>
            </div>-->
    </div>
  </div>
</template>

<script>
export default {
  name: "homeHeader",
  data() {
    return {
      langs: [{name: 'english', value: 'en'}, {name: '中文', value: 'cn'}],
      currentLang: this.$i18n.locale
    }
  },
  methods: {
    clickMenu(data) {
      this.$router.push(data)
    },
    changeLanguage(data) {
      this.$i18n.locale = data.value
      localStorage.setItem('locale', data.value)
      this.currentLang = data.value
    }

  },
  computed: {
    languageName() {
      const p = this.langs.filter(item => item.value === this.currentLang)[0].name
      return p
    }
  }
}
</script>

<style scoped lang="scss">
.head {
  display: flex;
  background-color: #06B176;
  width: 100%;
  //box-shadow: 1px 1px 5px #72767b;


  .logo {
    flex-shrink: 0;
    font-family: Helvetica;
    font-size: 34px;
    font-weight: 900;
    line-height: 60px;
    text-align: center;
    color: #045008;
    display: block;
    min-width: 150px;
    width: 200px;
    text-shadow: 2px 2px 1px #a5d4b7;

    //width: auto;
  }

  .menus {
    background-color: #06B176;
    flex-shrink: 0;
    flex-grow: 1;
    display: flex;
    line-height: 60px;
    //font-size: 40px;
    color: #fff;

    .menu {
      margin: 0 5px;
      padding: 0 10px;
      font-size: 22px;
      font-weight: 700;
      cursor: pointer;
      position: relative;

      .submenus {
        padding: 5px 0;
        display: none;
        z-index: 1000;
        position: absolute;
        top: 60px;
        left: 0px;
        background-color: #06B176;
        //padding: 0 10px;
        width: 200px;

        //font-size: 20px;
        border-radius: 5px;
        //border: 1px solid #00ff00;
        .submenu {
          font-size: 16px;
          line-height: 40px;
          padding: 0 10px;
          font-weight: 500;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;


          &:hover {
            background-color: #059463;
          }
        }
      }

      &:hover {
        background-color: #059463;

        .submenus {
          display: block;
        }
      }
    }
  }

  .right {
    margin: 0 20px;
    flex-shrink: 0;
    position: relative;

    background-color: #06B176;

    .mode {
      font-family: Helvetica;
      font-weight: 900;
      font-size: 15px;
      //color: #045008;
      //text-shadow: 2px 2px 1px #a5d4b7;
      line-height: 60px;
    }

    span {
      cursor: pointer;
      line-height: 60px;
      font-size: 18px;
      color: #fff;
    }

    .options {
      position: absolute;
      right: 0;
      display: none;
      background-color: #06B176;
      color: #fff;

      line-height: 30px;

      .option {
        cursor: pointer;
        padding: 0 10px;

        &:hover {
          background-color: #059463;
        }
      }

      &:hover {

      }
    }

    &:hover {
      .options {
        display: block;
      }
    }
  }
}
</style>