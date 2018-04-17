<template>
    <div>
        <loading margin="40px auto" v-show="loading" />

        <ul class="collapsible" data-collapsible="accordion">
            <li>
                <div class="collapsible-header" :style="headerThemeStyle"><i class="material-icons">settings</i>Filter By Site Category</div>
                <div class="collapsible-body" :style="headerThemeStyle">
                    <div class="filters row">
                        <div class="filter col xl2 l3 m4 s6" v-for="(active, key) in categories" @click.prevent="categories[key] = !categories[key]">
                            <input type="checkbox" class="filled-in" :checked="active" :id="'cat-'+key" checked="checked" />
                            <label :for="'cat-'+key">{{ key == 'Fund' ? 'Hedge ' + key : key }}</label>
                            <img :src="getPic(key)" v-bind:alt="pic" class = "icons">


                        </div>
                    </div>
                </div>
            </li>
        </ul>


        <div class="datatable-responsive">
        <datatable style="margin:0 0 40px 0" v-show="!loading"
            :header="false"
            :perPage="[10, 25, 50, 100, 500]"
            :defaultPerPage="100"
            :exportable="false"
            :printable="false"
            :searchable="false"
            :clickable="false"
            :columns="columns"
            :rows="rows"
        />
        </div>
</div>
</template>

<style scoped>
    .icons{
      margin-left: 10px;
      width: 20px;
      height: 20px;
      vertical-align: top;
      display: inline-block;
    }

   .filter{
      margin-bottom: 20px;
   }
   .filter:last-child{
      margin-bottom: 0;
   }
   .filled-in:checked+label:after{
       background-color: #21B0B9;
       border:2px solid #21B0B9;
   }
   .collapsible{
      margin-top: 40px;
      box-shadow: 0 1px 3px rgba(0,0,0,.2);
      border:none;

   }
   .collapsible-header{
     border-bottom: 1px solid rgba(0,0,0,.1);
   }
   .collapsible-body{
     border-bottom: 1px solid rgba(0,0,0,.1);
   }

   .dropdown-button{
       background-color: #21B0B9;
       min-width: 200px;
       margin-bottom: 15px;
   }

   @media (max-width:991px){
       .filters > .filter{
           flex-basis: 25%;
        }
   }
    @media (max-width:767px){
       .filters{
           margin-bottom: 15px;
       }
       .filters > .filter{
           flex-basis: 50%;
        }
        .dropdown-button{
            width: 100%;
        }

        .filter{
            margin-bottom: 40px;
        }
   }

</style>

<script>
    import Loading from './Loading'
    import DataTable from './DataTable/DataTable';
    import EventBus from '@/event-bus';
    //import DataTable from 'vue-materialize-datatable';
    export default {
        components: { Loading, datatable: DataTable },
        data() {
            return {
                loading: false,
                columns: [
                    { label: '#', field: 'rank', html:true, filterable: false, numeric:true, width: '60px'  },
                    { label: 'Name', field: 'name', html:true, filterable: false },
                    { label: 'Category', field: 'category', html:true, filterable: true },
                    { label: 'Description', field: 'description', filterable: false, hide_mob:true },
                ],
                rows: [],
                categories: {
                    Exchange: true,
                    Tracking: true,
                    News: true,
                    Games: true,
                    Coin: true,
                    Fund: true,
                    ICO: true,
                    Tools: true,
                },
                nightColor : '#1d2331',
                nightColorHeader : '#2a3143',
                textlightColor : '#000',
                textnightColor : '#fff',
                lightColor : '#fff',
                themeColor : '#fff',
                headerThemeColor : '#2a3143',
                nightIcon : "night",
            }
        },
        watch:{
            'categories.Exchange': function(){ this.triggerFilter() },
            'categories.Tracking': function(){ this.triggerFilter() },
            'categories.News': function(){ this.triggerFilter() },
            'categories.Games': function(){ this.triggerFilter() },
            'categories.Coin': function(){ this.triggerFilter() },
            'categories.Fund': function(){ this.triggerFilter() },
            'categories.ICO': function(){ this.triggerFilter() },
            'categories.Tools': function(){ this.triggerFilter() },
        },
        methods:{
            formatNum(n, c = 2){
                n = Math.round(n*100)/100;
                let d =  ".",
                    t = ",",
                    s = n < 0 ? "-" : "",
                    i = String(parseInt(n = Math.abs(Number(n) || 0).toFixed(c))),
                    j = (j = i.length) > 3 ? j % 3 : 0;
                return s + (j ? i.substr(0, j) + t : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + t) + (c ? d + Math.abs(n - i).toFixed(c).slice(2) : "");
            },
            getPic(index) {
              return '/static/img/' + index + this.nightIcon + '.png';
            },
            loadSites(){

                const catColors = {
                    Exchange: '#5E8FB9',
                    Tracking: '#5EB9B2',
                    News: '#60B95E',
                    Games: '#B95E82',
                    Coin: '#8FB93E',
                    Fund: '#FF9727',
                    ICO: '#A870DC',
                    Tools: '#d73a49',
                };
                let tmp=[];

                $.getJSON('static/sites.json', data => {
                    if(!Array.isArray(data)) return;


                    data.forEach( obj => {

                        let site = obj.site;
                        let alexa_rank = parseInt(obj.rank);
                        let name = obj.name;
                        let cat = obj.category;
                        let description = obj.description;
                        let date_launched = obj.date_launched;

                        if(site && alexa_rank && name && this.categories[cat])
                        tmp.push({
                            alexa_rank,
                            name: '<a target="_blank" style="color:#21B0B9" href="'+site+'">'+name+'</a>',
                            category: '<b style="color:'+catColors[cat]+';text-shadow:1px 1px 1px rgba(0,0,0,.05)">'+ (cat == 'Fund' ? 'Hedge ' + cat : cat) +'</b>' ,
                            description,
                            date_launched
                        })
                        else {
                            console.error('Skip:',site)
                        }
                    })

                    this.setRanck(tmp);
                });

            },
            setRanck(tmp){

                tmp.sort(function(a, b){
                    const keyA = a.alexa_rank,
                        keyB = b.alexa_rank;
                    if(keyA > keyB) return 1;
                    if(keyA < keyB) return -1;
                    return 0;
                });

                for(let i=0; i<tmp.length; i++){
                    tmp[i].rank = i + 1;
                }
                this.rows = tmp;
            },
            triggerFilter(){
                this.loadSites();
            }
        },
        mounted() {
            this.loadSites();
            $(this.$refs.filter_settings).dropdown('open');
            $('.collapsible').collapsible();
            var self = this;
            EventBus.$on('EVENT_NAME', function (payLoad) {
              self.themeColor = payLoad ? self.textnightColor : self.textlightColor;
              self.headerThemeColor = payLoad ? self.nightColorHeader : self.lightColor;
              self.nightIcon = payLoad ? "night" : "";
            });
        },
        computed: {
          themeStyle () {
            return {
              color: this.themeColor,
              paddingLeft: '10px'
            }
          },
          headerThemeStyle () {
            return {
              backgroundColor: this.headerThemeColor,
              color: this.themeColor,

            }
          },
        },
    }
</script>
