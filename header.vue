<template>
	<div>
		<transition name="custom-classes-transition" enter-active-class="animated slideInDown" leave-active-class="animated slideOutUp">
			<div class="mobile_menu_container" v-show="show_mobile_menu">
				<div class="opened_mobile_menu" >
					<div class="text-left" style="padding-bottom: 20px;border-bottom: 1px solid #fff;">
						<img src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1513273840666/mobile_menu_logo.png"  alt="">
						<img src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1513274154350/close_menu_icon.png" class="pull-right" alt="" style="margin-top: 10px;" id="close_menu" @click="show_mobile_menu = !show_mobile_menu">
					</div>
					<ul>
						<li v-for="item in menu_items">
							<a v-if="item.id=='magazine_link'" :href="magazine_url.image_url" target="_blank" :id="item.id" class="all_caps">Magazine</a>
							<router-link v-else :to="item.href" :id="item.id" class="all_caps">{{item.name}} </router-link>
						</li>
						<li>
							<router-link to="/map" id="map_link" class="all_caps"> Centre Map </router-link>
						</li>
					</ul>
					<div class="tel_num" v-if="property">
						<a :href="'tel:'+property.contact_phone">{{property.contact_phone}}</a>
					</div>
					<div class="m_menu_today_hour" v-if="todays_hours">
						Open Today: {{todays_hours.open_time | moment("h:mma", timezone)}} - {{todays_hours.close_time | moment("h:mma", timezone)}}
					</div>
					<div class="social_icons_menu show_phone text-center">
						<!--<p>Follow us on</p>-->
						<a href="https://www.facebook.com/CanyonCrestTowneCentre/" target="_blank">
						<img src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1512590798000/fb.png" class="" alt="">
						</a>
						<a href="https://twitter.com/shopcanyoncrest" target="_blank">
						<img src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1512590798000/twt.png" class="" alt="">
						</a>
						<a href="https://www.instagram.com/shopcanyoncrest/" target="_blank">
						<img src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1512590798000/insta.png" class="" alt="">
						</a>
					</div>
				</div>
			</div>
		</transition>
		<section class="content_container  position_relative menu_header">
			<div class="mobile_search visible_phone position_relative" v-show="show_mobile_search">
				<search-component :list="allStores" placeholder="Find Your Store" :suggestion-attribute="suggestionAttribute" v-model="mobile_search" @select="onOptionSelect" class="">
					<template slot="item" scope="option" class="manual">
						<article class="media">
							<p>
								<strong>{{ option.data.name }}</strong>
							</p>
						</article>
					</template>
				</search-component>
				<!--<input type="text" placeholder="Search" id="site_search_m" class="site_search" />-->
				<img src="//codecloud.cdn.speedyrails.net/sites/56c740936e6f642d56000000/image/jpeg/1456781961000/icon_close.jpg" class="close_search close_search_m" alt="search" @click="show_mobile_search = !show_mobile_search; mobile_search=''">
			</div>
			<div class="row header mobile_margin row header mobile_margin main_container menu_header_content">
				<div class="col-sm-4 col-xs-2 hidden_phone">
					<p class="">Follow us on</p>
					<div class="social_icons ">
						<a class="social_fb" href="https://www.facebook.com/CanyonCrestTowneCentre/" target="_blank"></a>
						<a class="social_twitter" href="https://twitter.com/shopcanyoncrest" target="_blank"></a>
						<a class="social_insta" href="https://www.instagram.com/shopcanyoncrest/" target="_blank"></a>
					</div>
				</div>
				<div class="col-sm-4 col-xs-7 text-center">
					<router-link to="/">
						<img class="site-logo hidden_phone" src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1518041199981/canyoncrestlogo600.png" alt="" style="width:220px; margin: auto;">
						<img class="site-logo show_phone" src="//codecloud.cdn.speedyrails.net/sites/5adf56af6e6f6412bb160000/image/png/1513356843893/canyon_crest_logo.png" alt="">
					</router-link>
				</div>
				<div class="col-sm-4 col-xs-5 text-center position_relative text_right">
					<span>
					<img src="//codecloud.cdn.speedyrails.net/sites/56c740936e6f642d56000000/image/png/1455901042000/icon_search.png" class="search_btn search_btn_m show_phone" alt="search" @click="show_mobile_search = !show_mobile_search">
					<img src="//codecloud.cdn.speedyrails.net/sites/56c740936e6f642d56000000/image/png/1455905827000/icon_menu.png" class="open_menu show_phone" alt="Menu"  id="open_menu"  @click="show_mobile_menu = !show_mobile_menu">
					</span>
					<img src="//codecloud.cdn.speedyrails.net/sites/56c740936e6f642d56000000/image/png/1455901042000/icon_search.png" class="search_btn search_btn_desk hidden_phone" alt="search" v-show="!is_searching">
					<img src="//codecloud.cdn.speedyrails.net/sites/56c740936e6f642d56000000/image/jpeg/1456781961000/icon_close.jpg" class="close_search close_search_desk hidden_phone" alt="search" v-show="is_searching" @click="mobile_search=''; is_searching = !is_searching">
					<!--<input type="text" placeholder="Search" id="site_search" class="site_search search_field hidden_phone" />-->
					<search-component :list="allStores" placeholder="Find Your Store" :suggestion-attribute="suggestionAttribute" v-model="mobile_search" @select="onOptionSelect" @input="currentlySearching" class="desktop_search hidden_phone">
						<template slot="item" scope="option" class="manual">
							<article class="media">
								<p>
									<strong>{{ option.data.name }}</strong>
								</p>
							</article>
						</template>
					</search-component>
				</div>
			</div>
			<div class="menu_container hidden_phone main_container menu_header_content" id="menubar" :class="{sticky : stickyMenu}">
				<ul class="stc_nav nav navbar-nav">
					<li v-for="item in menu_items" :id="item.parent_id" class=" nav_li">
						<a v-if="item.id=='magazine_link'" :href="magazine_url.image_url" target="_blank" :id="item.id" class="all_caps">Magazine</a>
						<router-link v-else :to="item.href" :id="item.id" class="all_caps">{{item.name}} </router-link>
					</li>
				</ul>
			</div>
		</section>
	</div>
</template>

<script>
    define(["Vue", "vuex", 'json!menu_items.json'], function (Vue, Vuex, MenuItems) {
        return Vue.component("header-component", {
            template: template, // the variable template will be injected,
            // data: function() {
            //     return {
            //         dataLoaded: false,
            //         menu_items: MenuItems
            //     }
            // },
            // computed: {
            //     ...Vuex.mapGetters([
            //         'property',
            //         'timezone',
            //         'getTodayHours'
            //     ]),
            //     todays_hours() {
            //         return this.getTodayHours;
            //     }
            // }
        });
    });
</script>