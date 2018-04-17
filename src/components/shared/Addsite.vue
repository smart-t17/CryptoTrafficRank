<template>
<form class="vue-form" @submit.prevent="submit" :style="themeStyle">
      <div>
        <label class="label" for="siteurl">Site Url</label>
        <input type="text" name="siteurl" id="siteurl" required="" v-model="siteurl">
      </div>
      <div>
        <label class="label" for="name">Site Name</label>
        <input type="text" name="name" id="name" required="" v-model="name.value">
      </div>
      <div>
        <label class="label" for="name">Category</label>
        <p class="select">
          <select class="budget" v-model="selection.member">
						<option value="0">Exchange</option>
						<option value="1">Tracking</option>
            <option value="2">News</option>
						<option value="3">Games</option>
            <option value="4">Coin</option>
						<option value="5">Hedge Fund</option>
            <option value="6">ICO</option>
            <option value="7">Tools</option>
					</select>
        </p>
      </div>
      </form>
</template>


<style scoped>

.vue-form div {
  position: relative;
  margin: 20px 0;
}
.vue-form .label {
  color: #94aab0;
  margin-bottom: 10px;
}
.vue-form .label {
  display: block;
}
.vue-form input,
.vue-form textarea,
.vue-form select,
.vue-form label {
  color: #2b3e51;
}
.vue-form input,
.vue-form textarea,
.vue-form select,
.vue-form legend {
  display: block;
  width: 100%;
  appearance: none;
}
.vue-form input,
.vue-form textarea,
.vue-form select {
  border: 1px solid #cfd9db;
  background-color: #ffffff;
  border-radius: 0.25em;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.08);
}

.vue-form .error-message {
  height: 35px;
  margin: 0px;
}
.vue-form .error-message p {
  background: #e94b35;
  color: #ffffff;
  font-size: 1.4rem;
  text-align: center;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  border-radius: 0.25em;
  padding: 16px;
}
.vue-form .error {
  border-color: #e94b35 !important;
}


</style>

<script>
import EventBus from '@/event-bus';
    export default {
    data: function() {
      return {
        siteurl: "",
        name: {
          value: "",
          valid: true
        },
        selection: {
          member: "0"
        },
        message: {
          text: 'Dear Mr. President,\n...',
          maxlength: 255
        },
        submitted: false,
        nightColor : '#1d2331',
        nightColorHeader : '#2a3143',
        textlightColor : '#000',
        textnightColor : '#fff',
        lightColor : '#fff',
        themeColor : '#000',
        headerThemeColor : '#fff',
      }
    },
    methods: {

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
        }
      },
      headerThemeStyle () {
        return {
          backgroundColor: this.headerThemeColor,

        }
      },
    },
  };
</script>
