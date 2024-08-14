<template>
  <div class="s-header-parent-main">
    <!-- <common-alert /> -->
    <common-top class="d-none d-sm-block" @showCatalog="showCatalogMenu = false" />
    <v-container class="s-header-container" style="padding-top: 6px; position: relative">
      <!-- <div class="py-4 mb-6 d">
        <div class="d-flex justify-space-between">
          <div>
            {{ $store.getters.getD }}
            <a class="s-header-phones pr-3" href="tel://88007759985">8 800 775 99 85</a>
            <a class="s-header-phones pr-3" href="tel://84952256285">8 495 225 62 85</a>
          <a class="s-header-phones" target="_blank" href="https://api.whatsapp.com/send/?phone=+79672823107"><span><img src="/icons/whats_small.svg" alt="" /></span></a>
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
      
        <div class="d-flex align-center justify-space-between">
          <div class="d-flex align-center ms-3">
            <div class="s-header-menu-btn" @click="showCatalogMenu = !showCatalogMenu">
              <i v-if="showCatalogMenu" class="fa fa-times"></i>
              <i v-else class="fa fa-bars"></i>
            </div>
            <div class="d-flex flex-row align-center">
              <img @click="OpenSearch" class="ms-5 s-header-search-icon" style="cursor: pointer" src="/icons/Search.svg" />
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
          <div class="s-header-main-logo d-flex align-center justify-center" style="position: relative; left: 25px" @click="showCatalogMenu = false">
            <nuxt-link to="/"><img src="/icons/logo.svg" /></nuxt-link>
          </div>
          <!-- <div class="wrapper"></div> -->
          <div class="s-header-actions-block d-flex align-center" @click="disableMenu($event)">
            <!-- <nuxt-link to="/" style="display: none;"><img src="/icons/profile.svg" alt="" /></nuxt-link> -->
            <nuxt-link to="/favorite" class="me-6 me-sm-6 s-main-link" id="hrefs" >
              <img src="/icons/like.svg" alt="" />
              <div v-show="countItemsFav !== 0" style="position: relative;">
                <div class="cartIcon">{{ countItemsFav }}</div>
              </div>
            </nuxt-link>
            <nuxt-link to="/compare" class="me-6 me-sm-6 s-main-link" id="hrefs">
              <img src="/icons/compare.svg" alt="" />
              <div v-show="countItemsCom !== 0" style="position: relative;">
               <div class="cartIcon">{{ countItemsCom }}</div>
              </div>
            </nuxt-link>
            <!-- <div style="width: 70px;" class="ml-6 d-inline-block"> -->
              <nuxt-link to="/cart" class="me-3 s-main-link">
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



    <v-dialog 
            v-model="showSearch" 
            max-width="810px"
            :fullscreen="$vuetify.breakpoint.xs" 
            persistent
            hide-overlay
            transition="dialog-transition"
            >
            <div class="s-popup h-100" ref="dialogSearch">
                <v-container class="d-flex flex-column h-100">
                  <div class="d-flex justify-space-between">
                    <form @submit.prevent="submitSearch()" class="ma-0 me-3 pa-0 w-100">
                      <div class="s-header-menu-search">
                        <v-text-field 
                          ref="inputSearch"
                          @input="submitSearch()" 
                          @keyup.enter="redirectSearch()"
                          v-model="search" 
                          single-line 
                          outlined 
                          dense 
                          label="Я хочу найти"
                        >
                          <template v-slot:append>
                            <img style="cursor: pointer" @click="submitSearch()" src="/icons/Search.svg" />
                          </template>
                        </v-text-field>
                      </div>
                    </form>
                    <div class="text-right" style="height: 36px">
                        <v-btn icon @click="showSearch = false"><img src="/icons/close_menu.svg" /></v-btn>
                    </div>
                </div>
                <div class="w-100 d-flex align-center justify-center flex-column">
                  <v-row
                  id="s-search-result" 
                  class="d-flex flex-column ma-0 mt-5 flex-nowrap pa-0 align-start"  
                  :class="{'visibility-active': showSearch && search.trim() && search.trim().length > 3}"
                  >
                  <v-col 
                  v-if="searchData?.category?.length === 0 && searchData?.brand?.length === 0 && searchData?.catalog?.length === 0"
                  class="d-flex flex-row align-center w-100 mb-0 pa-0"
                  >
                    <v-col class="col-3 pa-0">
                      <p class="mb-0" style="font-size: 14px !important;">Результаты поиска</p>
                    </v-col>
                    <v-col class="ma-0 pa-0">
                      <p class="mb-0" style="font-size: 14px !important;">Ничего не найдено</p>
                    </v-col>
                  </v-col>
                  <v-col
                    v-if="searchData?.category && searchData?.category.length > 0"
                    class="d-flex flex-row align-center w-100 mb-0 pa-0 mt-5"    
                  >
                    <v-col class="col-4">
                      <p class="mb-0" style="font-size: 14px !important;">Категории</p>
                    </v-col>
                    <v-col class="ma-0 d-flex flex-column">
                        <v-col
                        @click="[showSearch = false, search = '']"
                        v-for="el in searchData?.category"
                        :key="el._id" 
                        class="ma-0 pa-0"
                        >
                        <nuxt-link 
                        class="d-flex flex-row align-center w-100"
                        :to="el._source.isparent == 1 ? `/allcategories/${el._id}` : `/catalog/${el._id}`">
                          <div style="margin: 5px 10px">
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-if="el._source.images && el._source.images[0]"
                            :src="$config.baseImageURL + el._source.images[0]"
                            />
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-else src="/black-square.jpg" />
                          </div>
                          <p class="mb-0" style="font-size: 14px !important;">{{ el._source.name }}</p>
                        </nuxt-link>
                        </v-col>
                    </v-col>
                  </v-col>
                  <v-col
                    v-if="searchData?.brand && searchData?.brand.length > 0"
                    class="d-flex flex-row align-center w-100 pa-0 mt-5"    
                  >
                    <v-col class="col-4">
                      <p class="mb-0" style="font-size: 14px !important;">Бренды</p>
                    </v-col>
                    <v-col class="ma-0 pa-0 d-flex flex-column">
                        <v-col
                        @click="[showSearch = false, search = '']"
                        v-for="el in searchData?.brand"
                        :key="el._id" 
                        class="ma-0 pa-0"
                        >
                        <nuxt-link 
                        class="d-flex flex-row align-center w-100"
                        :to="'/catalog/search?q=' + el._source.name">
                          <div style="margin: 5px 10px">
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-if="el._source.images && el._source.images[0]"
                            :src="$config.baseImageURL + el._source.images[0]"
                            />
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-else src="/black-square.jpg" />
                          </div>
                          <p class="mb-0" style="font-size: 14px !important;">{{ el._source.name }}</p>
                        </nuxt-link>
                        </v-col>
                    </v-col>
                  </v-col>
                  <v-col
                    v-if="searchData?.catalog && searchData?.catalog.length > 0"
                    class="d-flex flex-row align-center w-100 pa-0 mt-5"    
                  >
                    <!-- <v-col class="col-3">
                      <p class="mb-0">Товары</p>
                    </v-col> -->
                    <v-col class="ma-0 pa-0 d-flex flex-column">
                        <v-col
                        @click="[showSearch = false, search = '']"
                        v-for="el in searchData?.catalog"
                        :key="el._id" 
                        class="ma-0 pa-0"
                        >
                        <nuxt-link 
                        class="d-flex flex-row align-center w-100"
                        :to="'/catalog/view/' + el._id">
                          <div style="margin: 5px 10px">
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-if="el._source.images && el._source.images[0]"
                            :src="$config.baseImageURL + el._source.images[0]"
                            />
                            <v-img 
                            contain 
                            style="width: 50px; height: 50px;"
                            v-else src="/black-square.jpg" />
                          </div>
                          <p class="mb-0 w-50" style="font-size: 14px !important;">{{ el._source.name }}</p>
                        <div class="wrapper mx-auto"></div>
                        <p v-if="el._source.price" style="font-size: 14px !important;" class="mb-0"><number :value="el._source.price" /> ₽</p>
                        <p v-else class="mb-0">Не указано</p>
                        </nuxt-link>
                        </v-col>
                    </v-col>
                  </v-col>
                      <div 
                      class="pa-0 mt-5"
                      v-if="searchData?.category?.length !== 0 || searchData?.brand?.length !== 0 || searchData?.catalog?.length !== 0">
                        <nuxt-link 
                        style="font-size: 14px !important;"
                        @click.native="[showSearch = false, search = '']"
                        class="underlined d-flex justify-start" :to="'/catalog/search?q=' + search">Посмотреть все результаты</nuxt-link>
                      </div>
                  </v-row>      
                </div>
              </v-container>
              </div>
            </v-dialog>
    <common-catalog-menu v-model="showCatalogMenu" @openSearch="OpenSearch" :items="catalogMenuItems" />
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import { debounce } from 'lodash';
export default {
  props: {
    catalogMenuItems: Array
  },
  data() {
    return {
      showCatalogMenu: false,
      showSearch: false,
      search: '',
      mass: [],
      searchData: [],
    }
  },
  watch: {
    showCatalogMenu: function () {
      if (this.showCatalogMenu) {
        this.showSearch = false;
      }
    },
    showSearch(){
      if(this.showSearch){
        this.$nextTick(() => {
          this.$refs.dialogSearch.scrollTop = 0
        })
      }
    }
  },
  methods: {
    OpenSearch() {
      this.showSearch = !this.showSearch;
      this.search = '';
      this.showCatalogMenu = false;
      window.scrollTo(0, 0);
  },
  disableMenu(e){
    // console.log(e.target);
    if(e.target.classList.contains('s-main-link') || e.target.closest('.s-main-link')){
      this.showCatalogMenu = false;
    }
  },
  redirectSearch(){
      if(this.search.trim() && this.search.trim().length > 3) {
        this.$router.push({ path: '/catalog/search', query: { q: this.search } });
        this.search = '';
        this.showSearch = false;
        this.$refs.inputSearch.$el.querySelector('input').blur();
      }
    },
  submitSearch: debounce(async function() {
      try {
        let alldata;
        if(this.search.trim() && this.search.trim().length > 2) {
          document.querySelector("#s-search-result").scrollTop = 0;
          document.querySelector("#s-search-result").scrollLeft = 0;
          // this.$router.push({ path: '/catalog/search', query: { q: this.search } })
          alldata = (await this.$axios.get(this.$config.baseURL + '/api/site/catalog/hits', { params: { q: this.search } })).data.data;
          // console.log(this.search);
          // console.log(alldata);
          this.searchData = alldata;
          this.showCatalogMenu = false;
        }
      } catch (error) {
        console.error(error)
      }
    }, 800),
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
.w-50{
  width: 50% !important;
}
#s-search-result{
  visibility: hidden;
  z-index: 1000000;
  background-color: #fff;
  right: 0 !important;
  a{
    border-bottom: 1px solid #DBDBDB;
  }
}
.visibility-active{
  visibility: visible !important;
}


.s-header-menu-search{
  .v-input__slot{
    background-color: white !important;
   }
}

.s-header-parent-main .cartIcon{
  background-color: black;
  border-radius: 50px;
  color: white;
  width: 16px;
  height: 16px;
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
.s-header-menu-search .v-text-field__details{
  display: none;
}
.s-header-parent-main > .wrapper{ 
  flex-grow: 1;
}

.s-header-main-logo {
  width: 150px;
  img{
    width: 90%;
  }
  position: relative;
  text-align: center;
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
  width: 16px;
  font-size: 20px;
  font-weight: bold;
  align-items: center;
  justify-content: center;
  display: flex;
}


@media screen and (max-width: 599px) {
  
  .s-header-container{
    padding-bottom: 6px !important;
    >div{
      margin-top: 4px !important;
    }
  }
  .s-header-parent-main .cartIcon{
    font-size: 9px !important;
    width: 13px !important;
    height: 13px !important;
    right: -10px !important;
  }
  .s-header-actions-block{
    img{
      width: 1em !important;
      height: 1em !important;
    }
  }
  .s-header-main-logo{
    left: 25px !important;
  }
  .s-header-search-icon{
    width: 1em !important;
  } 
  .s-header-menu-btn{
    i{
      font-size: 0.9em;
    }
  }
}
@media screen and (min-width: 600px) {
  .s-header-parent-main .cartIcon{
    right: -13px !important;  
  }
}
@media screen and (max-width: 500px) {
  .s-header-parent-main .cartIcon{
    font-size: 9px !important;
    width: 13px !important;
    height: 13px !important;
    right: -10px !important;
  }
  .s-header-actions-block{
    img{
      width: 1.5em;
      height: 1.5em;
    }
  }
}
@media screen and (max-width: 425px) {
  .s-header-main-logo{
    img{
      width: 75% !important;
    }
  }
}
@media screen and (max-width: 390px) {
  #form-search{
    margin: 10px 10px !important;
  }
  .s-header-actions-block{
    >a#hrefs{
      margin-right: 16px !important;
    }
  }
  .s-header-main-logo{
    left: 20px !important;
  }
}
</style>
