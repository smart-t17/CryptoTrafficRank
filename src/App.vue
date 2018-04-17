<template>
  <div id="app" class="app"  :style="themeStyle">
    <div class = "header" :style="headerThemeStyle">
    <span class="toggle">
      <toggle-button @change="onChangeEventHandler" :width="70" :value="true"
                     :labels="{unchecked: 'Day', checked: 'Night'}"/>
    </span>

    </div>
    <div class="logo">
      <img :src="logourl" alt="...">


    </div>
    <router-view/>
  </div>
</template>

<script>
import EventBus from './event-bus';
export default {
  name: 'App',
  data () {
    return {
      nightMode: true,
      nightColor : '#1d2331',
      nightColorHeader : '#2a3143',
      lightColor : '#fff',
      themeColor : '#1d2331',
      headerThemeColor : '#2a3143',
      logourl : "/static/img/logo_night.png",
    }
  },
  methods: {
    onChangeEventHandler(){
      this.nightMode = !this.nightMode;
      this.themeColor = this.nightMode ? this.nightColor : this.lightColor;
      this.headerThemeColor = this.nightMode ? this.nightColorHeader : this.lightColor;
      this.logourl = this.nightMode ? "/static/img/logo_night.png" :"/static/img/logo.png";
      EventBus.$emit('EVENT_NAME', this.nightMode);
    }

  },
  computed: {
    themeStyle () {
      return {
        backgroundColor: this.themeColor,

      }
    },
    headerThemeStyle () {
      return {
        backgroundColor: this.headerThemeColor,

      }
    },
  },
}
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
}

.toggle{
  margin-right: 30px;
  top: 18px;
  position: relative;
}
.logo{
  text-align: center;
  margin-top: 30px;
  font-size: 80px;
  color:#21B0B9;
}
.logo > img{
  width: 100%;
  height: auto;
  max-width: 200px;
}
.header{
  box-sizing: border-box;
  font-size: 14px;
  font-weight: 400;
  height: 60px;
  margin: 0;
  overflow: visible;
  position: relative;
  text-align: right;
  vertical-align: middle;
  width: 100%;
  z-index: 1000;
  border-bottom-width: 3px;
  border-bottom-style: solid;
  border-bottom-color: #fff;
  font-family: 'open-sanslight', sans-serif;
  border-bottom: none;
  border: 1px solid rgba(34,36,38,.15);
  box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
}
</style>
