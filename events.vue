<template>
    <div class=" main_container" id="events_container" v-if="dataLoaded"><!-- for some reason if you do not put an outer container div this component template will not render -->
        <h3 class="promotion_heading" v-if="property"> Exclusive Events at {{property.name}}</h3>
        <p class="exclusive_deals sub_title">Join us to celebrate and enjoy!</p>
        <div class="row">
            <div class="col-md-4 col-sm-4" v-for="promo in events">
                <hr class="show_phone">
                <div class="promo_list_container text_center">
                    <p class="top_promo_date">{{promo.start_date | moment("MMM D", timezone)}} - {{promo.end_date | moment("MMM D", timezone)}}</p>
                    <div class="promo_list_img_container">
                        <a :href="'/events/'+promo.slug" target="_blank">
                        <img :src="promo.image_url" class="promo_list_img">
                            
                        </a>
                    </div>
                    <p class="description_text">{{ promo.name }}</p>
                    <router-link :to="{ name: 'eventDetails', params: { id: promo.slug }}" class="animated_btn text_center">Read More</router-link>
                </div>
            </div>
        </div>
        <!--<div class="row">-->
        <!--    <full-calendar v-if="fcEvents" :events="fcEvents" locale="en" @eventClick="eventClicked" class="hidden_phone" :todaysDate="today"></full-calendar>-->
        <!--</div>-->
    </div>
    
</template>

<style>
  .center{
    text-align: center
  }
  .store-section a{
    color: #708090;
  }
  .top_promo_date{
      margin-top:10px;
  }
</style>

<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-meta"], function(Vue, Vuex, moment, tz, VueMoment, Meta) {
        Vue.use(Meta);
        return Vue.component("events-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    success_subscribe : false,
                    fcEvents : null,
                    dataLoaded: false,
                    today: null
                }
            },
            created () {
                this.$store.dispatch("getData", "events").then(response => {
                    this.dataLoaded = true;
                    var vm = this;
                    var temp_events = [];
                    var today = moment().tz(vm.timezone);
                    _.forEach(this.processedEvents, function(value, key) {
                        
                        webDate = moment(value.show_on_web_date).tz(vm.timezone)
                        if (today.format() >= webDate.format()) {
                            var event = {};
                            event.title = value.name;
                            var start_date = moment(value.start_date).tz(vm.timezone)
                            var end_date = moment(value.end_date).tz(vm.timezone)
                            event.start = start_date.format("YYYY-MM-DD");
                            if( start_date.format("YYYY-MM-DD") !== end_date.format("YYYY-MM-DD") ){
                                event.end = end_date.format("YYYY-MM-DD");
                            }
                            
                            event.cssClass = "event_color";
                            event.slug = value.slug;
                            temp_events.push(event);
                        }
                    });
                    this.fcEvents = temp_events;
                    this.today = today.toDate();
                }, error => {
                    console.error("Could not retrieve data from server. Please check internet connection and try again.");
                    this.$router.replace('/');
                });
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'processedEvents'
                ]),
                events () {
                    var this_events = this.processedEvents;
                    var vm = this;
                    var today = moment().tz(vm.timezone);
                    _.forEach(this_events, function(event, key) {
                        
                        webDate = moment(event.show_on_web_date).tz(vm.timezone)
                        if (today.format('DMY') >= webDate.format('DMY')) {
                           if(event.store != null && event.store != undefined && _.includes(event.store.store_front_url_abs, 'missing')){
                                event.store.store_front_url_abs =  vm.property.default_logo_url;
                            }
                            else if (event.store == null && event.store == undefined) {
                                event.store = {};
                                event.store.store_front_url_abs =  vm.property.default_logo_url;
                            }
                            
                            if(event.image_url != null && event.image_url != undefined && _.includes(event.image_url, 'missing')){
                                event.image_url =  event.store.store_front_url_abs
                            }
                        }
                    });
                    return this_events;
                }
            },
            methods : {
                eventClicked(event) {
                    this.$router.push('/events/'+event.slug)
                }  
            },
        });
    });
</script>
