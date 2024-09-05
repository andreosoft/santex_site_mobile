<template>
  <transition name="fade">
    <div v-if="value" class="s-menu-catalog">
      <v-container style="position: relative; height: 100%">
        <common-divider1/>
        <div v-if="activeEl>=0">
            <v-row class="ma-0">
              <v-col cols="12">
                <div class="s-catalog-links">
                  <v-row class="ma-0">
                    <v-row class="d-flex flex-row align-center ma-0 mb-5">
                      <v-col class="col-1 pa-0">
                        <img class="pointer" @click="activeEl = -1" src="/icons/arrow_left_black.svg" alt="">
                      </v-col>
                      <v-col class="col-11 pa-0 d-flex flex-wrap">
                        <h3 class="ms-2 pa-0">
                          {{ items[activeEl].name }}
                        </h3>
                      </v-col>
                    </v-row>
                    <v-col class="pa-0 d-flex flex-row flex-wrap col-12">
                      <div class="s-catalog-links-el" v-for="(el, i) in items[activeEl].content" :key="i" @click="$emit('input')">
                        <nuxt-link :to="'/catalog/' + el.id" >{{ el.name }}</nuxt-link>
                      </div>
                    </v-col>
                  </v-row>
                  <div class="s-catalog-brends">
                    <div class="pb-4"><b>Бренды</b></div>
                    <v-row class="s-catalog-brends-links" style="padding: 5px;">
                      <v-col
                        cols="4"
                        style="padding: 5px;"
                        v-for="(el, i) in items[activeEl].brands"
                        :key="i"
                        @click="$emit('input')"
                      >
                        <nuxt-link 
                        class="underlined" 
                        style="color: #636363 !important;" 
                        :to="`/catalog/brands?filters=%7B%22brand%22%3A%5B%22${encodeURIComponent(el)}%22%5D%7D&f=%7B%7D&page=0`"
                        >
                          {{ el }}
                        </nuxt-link>
                      </v-col>
                    </v-row>
                  </div>
                </div>
              </v-col>
              <v-col cols="3">
                <div class="text-center pt-10" v-if="items[activeEl].ads">
                  <v-img :src="items[activeEl].ads.img" />
                  <p class="pt-4">
                    <b>{{ items[activeEl].ads.name }}</b>
                  </p>
                  <div @click="$emit('input')">
                    <nuxt-link class="underlined" :to="items[activeEl].ads.url">Подробнее</nuxt-link>
                  </div>
                </div>
              </v-col>
            </v-row>
        </div>
        <div v-else>
          <v-row class="flex-column flex-sm-row align-sm-center ma-0">
            <v-col id="numb-head" class="col-12 col-sm-6 d-flex align-center justify-space-between">
                <a @click="$emit('input')" class="s-header-phones" href="tel://88007759985">8 800 775 99 85</a>
                <a @click="$emit('input')" class="s-header-phones" href="tel://84952256285">8 495 225 62 85</a>
              <a @click="$emit('input')" class="s-header-phones me-5" target="_blank" href="https://api.whatsapp.com/send/?phone=79672823107"><span><img class="icons-head" src="/icons/whats_small.svg" alt="" /></span></a>
            </v-col>
            <v-col class="col-12 col-sm-6 d-flex align-end align-sm-center justify-space-between right-up-menu" style="border-left: 1px solid #e5e5e5">
              <div class="d-block d-sm-none">
                <nuxt-link to="/" class="d-flex flex-column flex-sm-row align-center">
                  <img class="mb-2 mb-sm-0 icons-head" src="/icons/location.svg" alt="" />
                  <s-select-city v-model="currentCity" />
                </nuxt-link>
              </div>
              <!-- <div>
                <div>
                  <img src="/icons/location.svg" style="height: 14px;" class="pr-2"
                    />
                  <span style="color: #aaa; font-size: 14px" class="pr-2">Ваш город:</span>
                  <s-select-city v-model="currentCity" />
                </div>
              </div> -->
              <div @click="$emit('input')">
                <nuxt-link class="d-flex flex-column flex-sm-row align-center s-cat-compare" to="/compare">
                  <img class="mb-2 mb-sm-0 icons-head" src="/icons/compare.svg" alt="" />
                  <div v-show="countItemsCom !== 0" style="position: relative;">
                    <div class="cartIcon">{{ countItemsCom }}</div>
                  </div>
                  <span class="ms-sm-4">Сравнение</span>
                </nuxt-link>
              </div>
              <div @click="$emit('input')" class="order-sm-first ms-sm-5">
                <nuxt-link to="/" class="d-flex flex-column flex-sm-row align-center">
                  <img class="mb-2 mb-sm-0 icons-head" src="/icons/profile.svg" alt="" />
                  <span class="ms-sm-4">Личный кабинет</span>
                </nuxt-link>
              </div>
            </v-col>
          </v-row>
          <v-row class="ma-0">
            <form @click="submitSearch()" style="width: 100%" class="s-header-search mt-4 ms-3 me-3">
              <div class="s-header-menu-search">
                <v-text-field v-model="search" single-line outlined dense label="Я хочу найти">
                  <template v-slot:append>
                    <img style="cursor: pointer" @click="submitSearch()" src="/icons/Search.svg" />
                  </template>
                </v-text-field>
              </div>
            </form>
          </v-row>
          <div class="s-menu-catalog-left">
            <div class="pt-10">
              <div class="s-catalog-links">
                <v-row class="ma-0 flex-column">
                  <h3 class="mt-5 ms-5">Каталог</h3>
                  <v-row class="ma-0">
                    <v-col class="s-item col-sm-12 d-flex flex-column flex-sm-row flex-wrap">
                      <div v-for="(el, i) in items" class="s-catalog-links-el" @click="activeEl = i" :key="i">
                        <span class="s-catalog-links-el-icon"><img/></span>
                        <!-- <span class="s-catalog-links-el-icon"><img :src="el.icon"/></span> -->
                        <span>
                          {{ el.name }}
                        </span>
                      </div>
                    </v-col>
                  </v-row>
                </v-row>
              </div>
            </div>
          </div>
          <common-divider1/>
          <div class="s-menu-catalog-left">
            <div class="pt-sm-8 ps-5">
              <div class="s-catalog-links">
                <v-row class="ma-0 flex-column">
                  <v-row class="ma-0">
                    <v-col class="col-6 d-flex flex-column ">
                      <div class="pt-2 s-catalog-down-links" @click="$emit('input')">
                        <nuxt-link to="/brends">Бренды</nuxt-link></div>
                      <div class="pt-2 s-catalog-down-links" @click="$emit('input')">
                        <nuxt-link to="/blog">Блог</nuxt-link></div>
                      <div class="pt-2 s-catalog-down-links" @click="$emit('input')">
                        <nuxt-link to="/designers">Наши дизайнеры</nuxt-link></div>
                      <div class="pt-2 s-catalog-down-links" @click="$emit('input')">
                        <nuxt-link to="/consulting">Запись на консультацию</nuxt-link></div>
                      <div class="pt-2 s-catalog-down-links" @click="$emit('input')">
                        <nuxt-link to="/promote/1">Распродажа</nuxt-link></div>
                    </v-col>
                    <v-col class="col-6 d-flex flex-column">
                      <div @click="$emit('input')" class="pt-2 s-catalog-down-links" v-for="(el, i) in links" :key="i">
                        <nuxt-link :to="el.to">{{ el.title }}</nuxt-link></div>
                    </v-col>
                  </v-row>
                </v-row>
              </div>
            </div>
          </div>
          <common-divider1/>
          <v-row class="common-links s-bottom d-flex flex-row align-center ps-4 mb-2" id="common-links">
            <v-col @click="$emit('input')" class="col-6 text-center">
              <nuxt-link @click="$emit('input')" to="/">© SantehKomfort Elite, {{ new Date().getFullYear() }}</nuxt-link> 
            </v-col>
            <v-col @click="$emit('input')" class="col-6 text-center">
              <nuxt-link @click="$emit('input')" to="/publicOffer">Публичная оферта</nuxt-link> 
            </v-col>
            <v-col @click="$emit('input')" class="col-6 text-center">
              <nuxt-link @click="$emit('input')" to="/privacy">Политика конфиденциальности</nuxt-link>
            </v-col>
            <v-col @click="$emit('input')" class="col-6 text-center">
              <nuxt-link @click="$emit('input')" to="/acceptRules">Правила приемки</nuxt-link> 
            </v-col>
            <v-col @click="$emit('input')" class="col-6 text-center">
              <nuxt-link @click="$emit('input')" to="/userAgreement">Пользовательское соглашение</nuxt-link>
            </v-col>
          </v-row>
        </div>
      </v-container>
    </div>
  </transition>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
  props: {
    value: Boolean,
    items: Array,
  },
  watch: {
    value: function(){
      if(this.value){
        this.activeEl = -1
      }
    }
  },
  data() {
    return {
      currentCity: "moscow",
      activeEl: -1,
      showSearch: false,
      search: '',
      links: [
        {
          to: "/pages/about",
          title: "О компании",
        },
        {
          to: "/pages/delivery",
          title: "Доставка",
        },
        {
          to: "/pages/payment",
          title: "Оплата",
        },
        {
          to: "/pages/contacts",
          title: "Контакты",
        },
        {
          to: "/pages/garanty",
          title: "Гарантия и возвраты",
        }
      ],
    };
  },
  methods: {
    submitSearch() {
      // this.$router.push({ path: '/catalog/search', query: { q: this.search } })
      this.showSearch = false
      this.$emit('openSearch')
    },
  },
  computed: {
    ...mapGetters ({
      countItemsCom: 'compare/countItems'
    })
  }
};
</script>

<style lang="scss">
  .right-up-menu{
    .cartIcon{
      right: -18px;
    }
  }
  .s-menu-catalog{
    .s-common-divider1{
      margin: 0 !important;
    }
  }
  @media screen and (max-width: 740px) {
    .right-up-menu{
      .icons-head{
        width: 1.2em;
      }
      span {
        font-size: 14px !important;
      }
    }
    #numb-head{
      .icons-head{
        width: 1.2em;
      }
      .s-header-phones{
        font-size: 14px !important;
      }
    }
  }
  @media screen and (min-width: 425px) and (max-width: 600px) {
    #numb-head{
      justify-content: flex-start !important;
      align-items: end !important;
      a:not(:first-child){
        margin-left: 20px !important;
      }
    }
  }
  @media screen and (max-width: 634px) {
    .right-up-menu{
      .icons-head{
        width: 1.1em;
      }
      span {
        font-size: 12px !important;
      }
    }
    #numb-head{
      .icons-head{
        width: 1.1em;
      }
      .s-header-phones{
        font-size: 12px !important;
      }
    }
  }
  @media screen and (max-width: 600px) {
    
    .right-up-menu{
      border-left: unset !important;
    }
  }
  @media screen and (max-width: 599px) {
    .right-up-menu{
      .cartIcon{
        right: -35px !important;
        top: -25px;
      }
      .icons-head{
        width: 1.3em;
      }
      span {
        font-size: 16px !important;
      }
    }
    #numb-head{
      .icons-head{
        width: 1.3em;
      }
      .s-header-phones{
        font-size: 16px !important;
      }
    }
  }
@media screen and (max-width: 505px){
  .common-links {
    div{
      padding: 0 !important;
      padding-bottom: 20px !important;
      a{
        padding: 0 !important;
      }
    }
  }
  .right-up-menu{
    .icons-head{
      width: 1.2em;
    }
    span {
      font-size: 14px !important;
    }
  }
  #numb-head{
    .icons-head{
      width: 1.2em;
    }
    .s-header-phones{
      font-size: 14px !important;
    }
  }
  .right-up-menu{
    border-left: unset !important;
  }
}
@media screen and (max-width: 425px) {
  
}
@media screen and (max-width: 375px){
  .s-catalog-links{
    div>h3{
      font-size: 27px !important;
      padding-bottom: 0 !important;
    }
  }
  .s-catalog-links-el{
    span{
      font-size: 14px !important;
    }
  }
  .s-catalog-down-links{
    padding-top: 0 !important;
    a{
      font-size: 14px !important;
    }
  }
  #common-links{
    a{
      font-size: 11px !important;
    }
  }
}
  .icons-head{
    width: 1.5em;
  }
</style>