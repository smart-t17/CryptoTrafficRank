<template>
  <div class="container">

    <div class="title">
      <h1>A list of cryptocurrency sites ranked by popularity</h1>
      <h2 :style="themeStyle">Rankings are updated every 24 hours</h2>
    </div>

    <app-sites></app-sites>


    <div class="footer">
      <div class="verticalcenter"> <a class="modal-trigger" href="#terms_of_service" :style="themeStyle">Terms of Service</a> </div>
      <div class="verticalcenter"> <a class="modal-trigger" href="#add_a_site" :style="themeStyle">Contact Us</a></div>
      <a class="verticalcenter" href = "https://www.facebook.com/Cryptotrafficrank-934731206685920"> <i class="fa fa-facebook-square fa-2x" :style="themeStyle"></i></a>
      <a class="verticalcenter" href = "https://www.facebook.com/Cryptotrafficrank-934731206685920"> <i class="fa fa-tumblr-square fa-2x" :style="themeStyle"></i></a>

    </div>


    <div id="terms_of_service" ref="terms_of_service" class="modal modal-fixed-footer" :style="themeStyle">
      <div class="modal-content">
        <h4 style="font-size:26px">Terms of Service</h4>
          <app-terms></app-terms>
      </div>
      <div class="modal-footer" :style="themeStyle">
        <a href="#" @click.prevent class="modal-action modal-close waves-effect waves-green btn-flat btn-submit" style="color='#21B0B9'">Agree</a>
      </div>
    </div>

    <div id="add_a_site" ref="add_a_site" class="modal modal-fixed-footer modal-contact" :style="themeStyle">
      <div class="modal-content">
        <h4 style="font-size:26px">Add A Site</h4>
          <app-addsite></app-addsite>
      </div>
      <div class="modal-footer" :style="themeStyle">
        <a href="#" @click.prevent class="modal-action modal-close waves-effect waves-green btn-flat  btn-submit">Submit</a>
      </div>
    </div>

  </div>
</template>


<style scoped>
  .title, .tos{
    text-align: center;
  }
  .verticalcenter {
    margin-bottom : 20px;
    margin-left : 20px;

    display: inline-block;
    vertical-align: middle;
  }
  .btn-submit {
  color:#21B0B9;
  }
  .modal-contact {
  width: 30%;
  height: 50%;
  }
  .footer {
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
  color : 'white';
  z-index: 1000;
  border-bottom-width: 3px;
  border-bottom-style: solid;
  border-bottom-color: #fff;
  font-family: 'open-sanslight', sans-serif;
  border-bottom: none;
  border: 1px solid rgba(34,36,38,.15);
  box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
  background: "#2a3143";
  padding : 20px;
  }
  .title h1{
    color:#21B0B9;
    font-size: 18px;
    margin: 15px 0;
  }
  .title h2{
    color:#000000;
    font-size: 18px;
    margin: 15px 0;
  }
  .tos{
    margin-bottom: 20px;
  }
  .tos a{
    color:#21B0B9;
    font-size: 22px;
  }
</style>

<script>
  import Sites from './shared/Sites';
  import Terms from './shared/Terms';
  import Addsite from './shared/Addsite';
  import EventBus from '@/event-bus';
  export default {
    components: { appSites: Sites, appTerms: Terms , appAddsite: Addsite},
    data () {
      return {
        nightColor : '#1d2331',
        nightColorHeader : '#1d2331',
        textlightColor : '#000',
        textnightColor : '#fff',
        lightColor : '#fff',
        themeColor : '#fff',
        headerThemeColor : '#1d2331',
      }
    },
    mounted() {
      $(this.$refs.terms_of_service).modal();
      $(this.$refs.add_a_site).modal();
      var self = this;
      EventBus.$on('EVENT_NAME', function (payLoad) {
        self.themeColor = payLoad ? self.textnightColor : self.textlightColor;
        self.headerThemeColor = payLoad ? self.nightColorHeader : self.lightColor;
      });
    },
    computed: {
      themeStyle () {
        return {
          color: this.themeColor,
          backgroundColor: this.headerThemeColor,
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
