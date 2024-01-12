<template>
  <div class="s-header-parent-main">
    <common-top class="d-none d-sm-block" />
    <v-container>
      <!-- <div class="py-4 mb-6 d">
        <div class="d-flex justify-space-between">
          <div>
            {{ $store.getters.getD }}
            <a class="s-header-phones pr-3" href="tel://88007759985">8 800 775 99 85</a>
            <a class="s-header-phones pr-3" href="tel://84952256285">8 495 225 62 85</a>
          <a class="s-header-phones" target="_blank" href="https://api.whatsapp.com/send/?phone=89672823107"><span><img src="/icons/whats_small.svg" alt="" /></span></a>
          </div>
          <div class="s-header-actions-block">
            <nuxt-link to="/" style="display: none;"><img src="/icons/profile.svg" alt="" /></nuxt-link>
            <nuxt-link class="ml-6" to="/favorite">
              <img src="/icons/like.svg" alt="" />
              <div v-show="countItemsFav !== 0" class="cartIcon">{{ countItemsFav }}</div>
            </nuxt-link>
            <nuxt-link class="ml-6" to="/compare" style="display: none;">
              <img src="/icons/compare.svg" alt="" />
              <div v-show="countItemsCom !== 0" class="cartIcon">{{ countItemsCom }}</div>
            </nuxt-link>
              <nuxt-link class="ml-6 me-3" to="/cart">
                  <img src="/icons/basket.svg" alt="compare" />
                  <div v-show="countItems !== 0" class="cartIcon">{{ countItems }}</div>
              </nuxt-link>
          </div>
        </div>
      </div> -->
      
        <div class="d-flex align-center justify-space-between mb-4">
          <div class="d-flex align-center ms-3">
            <div class="s-header-menu-btn" @click="showCatalogMenu = !showCatalogMenu">
              <i v-if="showCatalogMenu" class="fa fa-times"></i>
              <i v-else class="fa fa-bars"></i>
            </div>
            <div class="d-flex flex-row align-center">
                  <transition name="fade">
                  <form v-if="showSearch"  @submit.prevent="submitSearch()" class="ma-0 ms-5 pa-0 form-search" style="width: 180px; position: absolute; top: 50px; z-index: 100; overflow: hidden">
                    <div class="s-header-menu-search">
                      <v-text-field v-model="search" single-line outlined dense label="Я хочу найти">
                        <template v-slot:append>
                          <img style="cursor: pointer" @click="submitSearch()" src="/icons/Search.svg" />
                        </template>
                      </v-text-field>
                    </div>
                  </form>
                </transition>
            <img @click="showSearch = !showSearch" class="ms-5 s-header-search-icon" style="cursor: pointer" src="/icons/Search.svg" />
            </div>
            </div>
          <!-- <div class="s-header-menu-links pt-2">
            <nuxt-link to="/brends">Бренды</nuxt-link>
            <nuxt-link to="/blog">Блог</nuxt-link>
            <nuxt-link to="/designers">Наши дизайнеры</nuxt-link>
            <nuxt-link to="/consulting">Запись на консультацию</nuxt-link>
            <nuxt-link to="/promote/1"><img src="/icon-discont.png" class="pr-1" />Распродажа</nuxt-link>
          </div> -->
          <!-- <div class="wrapper"></div> -->
          <div class="s-header-main-logo d-flex align-center justify-center">
            <nuxt-link to="/"><img src="/icons/logo.svg" /></nuxt-link>
          </div>
          <!-- <div class="wrapper"></div> -->
          <div class="s-header-actions-block d-flex align-center">
            <!-- <nuxt-link to="/" style="display: none;"><img src="/icons/profile.svg" alt="" /></nuxt-link> -->
            <nuxt-link to="/favorite">
              <img src="/icons/like.svg" alt="" />
              <div v-show="countItemsFav !== 0" style="position: relative;">
                <div class="cartIcon">{{ countItemsFav }}</div>
              </div>
            </nuxt-link>
            <nuxt-link to="/compare" style="display: none;">
              <img src="/icons/compare.svg" alt="" />
              <div v-show="countItemsCom !== 0" class="cartIcon">{{ countItemsCom }}</div>
            </nuxt-link>
            <!-- <div style="width: 70px;" class="ml-6 d-inline-block"> -->
              <nuxt-link to="/cart" class="ms-7 me-3">
                  <img src="/icons/basket.svg" alt="compare" />
                  <div v-show="countItems !== 0" style="position: relative;">
                    <div class="cartIcon">{{ countItems }}</div>
                  </div>
              </nuxt-link>
            <!-- </div> -->
          </div>
          <!-- <form class="s-header-search" @submit.prevent="submitSearch()">
            <div class="s-header-menu-search">
              <v-text-field v-model="search" single-line outlined dense label="Я хочу найти">
                <template v-slot:append>
                  <img style="cursor: pointer" @click="submitSearch()" src="/icons/Search.svg" />
                </template>
              </v-text-field>
            </div>
          </form> -->
        </div>
    </v-container>
    <common-catalog-menu v-model="showCatalogMenu" :items="catalogMenuItems" />
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
  props: {
    catalogMenuItems: Array
  },
  data() {
    return {
      showCatalogMenu: false,
      showSearch: false,
      search: '',
      mass: []
    }
  },
  watch: {
    showCatalogMenu: function () {
      if (this.showCatalogMenu) {
        this.showSearch = false;
      }
    }
  },
  methods: {
    submitSearch() {
      this.$router.push({ path: '/catalog/search', query: { q: this.search } });
      this.showSearch = false;
    },
  },
  computed: {
    ...mapGetters ({
      countItems: 'cart/countItems',
      countItemsFav: 'favorite/countItems',
      countItemsCom: 'compare/countItems'
    })
  }
}
</script>



<style lang="scss">
.s-header-parent-main .cartIcon{
  background-color: black;
  border-radius: 50px;
  color: white;
  width: 18px;
  height: 18px;
  font-size: 10px;
  padding-top: 1px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -20px;
  right: -20px;
}

.s-header-parent-main > .v-input__control{
  background-color: #FFF;
  margin: 0 !important;
  height: 40px;
}
.s-header-parent-main > .v-text-field__details{
  display: none;
}
.s-header-parent-main > .wrapper{ 
  flex-grow: 1;
}

.s-header-main-logo {
  img{
    width: 80%;
  }
  height: 0;
  position: relative;
  text-align: center;
  a {
    // position: absolute;
  }
}

.s-header-phones {
  font-size: 16px;
  font-weight: bold;
}

.s-header-menu-links {
  margin-left: 80px;

  a {
    padding: 0 20px;
    font-size: 16px;
    font-weight: bold;
  }
}

.s-header-menu-btn {
  vertical-align: middle;
  cursor: pointer;
  color: black !important;
  font-size: 20px;
  font-weight: bold;
  align-items: center;
  justify-content: center;
  display: flex;
  i{
    width: 20px;
  }
}

@media screen and (max-width: 700px) {
  .form-search{
    top: 100px !important;
  }
}
@media screen and (max-width: 600px) {
  .form-search{
    top: 50px !important;
  }
}
@media screen and (min-width: 600px) {
  .s-header-parent-main .cartIcon{
  right: -13px !important;  
  }
}
@media screen and (max-width: 425px) {
  .s-header-main-logo{
    img{
      width: 70% !important;
    }
  }
}
</style>
