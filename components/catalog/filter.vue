<template>
  <div class="parent">
    <!-- <div>
      <v-btn @click="onUpdateData()">Показать</v-btn>
    </div> -->
    <v-row id="filter-head" class="d-flex flex-row align-center justify-end">
      <div @click="$emit('filter-small', $event)" class="d-flex align-center justify-center" style="cursor: pointer">
        <i style="width: 100%" class="fa fa-times"></i>
      </div>
    </v-row>
    
    <h3 class="pa-0 ma-0 mb-8 "><b>Фильтр</b></h3>
    <catalog-filterResult :loading="loading" class="d-none d-md-block" @filterResult="onUpdateData" :locationRes="dy" :resultData="resultData"/>
    <div class="space-check">
      <catalog-price @location="locationResult" title="Цена, руб." v-model="dataPrice"
        :max="filters?.price?.max_price" 
        :min="filters?.price?.min_price" />
        <catalog-brands 
        v-if="filters.brands && filters.brands.length > 1" @location="locationResult" 
        :params="filterBrand" v-model="dataF.brand"  />
        <v-divider v-if="filters.brands && filters.brands.length > 1" class="my-4" />
        <catalog-collections class="mt-4" v-if="filters.collections && filters.collections.length > 1"
        @location="locationResult" :params="filterColl" v-model="dataF.collection" />
        <v-divider v-if="filters.collections && filters.collections.length > 1" class="my-4" />
        <catalog-categories class="mt-4"
        v-if="filters.categories"
        @location="locationResult"
        :params="filters.categories"
        v-model="dataF.category_id"
        />
        <!-- {{ filters }} -->
        <v-divider v-if="filters.categories && filters.categories.length > 1" class="my-4" />
    </div>
    <div v-for="(el, i) in filters.filters" :key="i">
      <div>
        <div v-if="el.type == 2" class="space-check">
          <catalog-price v-if="el.max != el.min" @location="locationResult" :title="el.name"
            v-model="dataF[el.filters_id]" :max="el.max" :min="el.min" />
        </div>
        <div v-else class="space-check">
          <catalog-check1 v-if="el.filters_data.length > 1" v-model="dataF[el.filters_id]" @location="locationResult"
            :title="el.name" :params="el.filters_data" />
          <v-divider v-if="el.filters_data.length > 1" class="my-4" />
        </div>
      </div>
    </div>
    <v-btn @click="onUpdateData" dark class="s-btn-cart s-btn-text w-100 d-flex d-md-none">       
     Показать
     <number class="ml-2" :value="resultData?.count" />
  </v-btn>
    <v-btn @click="toggleFunction($event)" class="s-btn-cart s-btn-text s-clear-filter w-100 mt-3 d-flex d-md-none">       
     Сбросить фильтр
     <img class="ml-2" src="/icons/Close catalog.svg" alt="">
  </v-btn>
  </div>
</template>

<script>
import {debounce} from "lodash"
export default {
  props: {
    value: Object,
    filters: Object,
    filterSmall: {
      type: Boolean,
      default: false
    },
    activeFilters: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      loading: false,
      dataF: {
        brand: [],
        category_id: [],
        collection: []
      },
      dataPrice: [],
      dy: 0,
      resultData: {},
      disTop: 0
    };
  },
  created() {
    this.initValueFilters();
  },
  mounted() {
    function getDistanceToDocumentTop(element) {
          let distance = 0;
          while (element) {
            distance += element.offsetTop;
            element = element.offsetParent;  // Переходим к родительскому элементу для следующей итерации
          }
          return distance;
        }
        this.disTop = getDistanceToDocumentTop(document.querySelector('.parent > .s-filterResult')) + 30;
        // console.log(this.disTop);
    if (this.activeFilters && Object.keys(this.activeFilters).length > 0) {
      this.updateFiltersData(this.activeFilters);
    }
    this.value?.price ? this.dataPrice = this.value?.price : this.dataPrice = [];
  },
  watch:{
    filterSmall: function(){
      this.filterSmall ? document.querySelector('.s-filter-small').scrollTo(0, 0) : ''
    },
    filters: function(){
        //  try {
           this.initValueFilters();
          //  console.log('filters function')
        //  }
        //  catch (error){
        //    console.error(error)
        //  }
    },
    value: function(){
      // try {
        // console.log('value function')
        this.initValueFilters();
        this.value.brand ? this.dataF.brand = this.value.brand : this.dataF.brand = [];
        this.value.category_id ? this.dataF.category_id = this.value.category_id : this.dataF.category_id = [];
        this.value.collection ? this.dataF.collection = this.value.collection : this.dataF.collection = [];
        // console.log("Инициализировано")
      // }
      // catch (error){
      //   console.error(error)
      // }
    },
    "value.price"() {
      // console.log(this.value.price ? this.value.price : this.dataPrice);
      this.value.price ? this.dataPrice = this.value.price : this.dataPrice = [];
    }
  },
  computed: {
    filterBrand(){
      return this.filters.brands
    },
    filterColl(){
      return this.filters.collections
    }
  },
  methods: {
    toggleFunction(e) {
      this.clearFilters();
      this.locationResult(e.target);
    },
    initValueFilters() {
      this.dataF = {
        brand: this.value.brand ? this.value.brand : [],
        collection: this.value.collection ? this.value.collection : [],
        category_id: this.value.category_id ? this.value.category_id : [],
      };
      this.dataPrice = this.value.price ? this.value.price : [];
      for (const key in this.filters.filters) {
        let f = [];
        if (this.value?.f[this.filters.filters[key]["filters_id"]]){
          f = this.value.f[this.filters.filters[key]["filters_id"]];
        } 
        // console.log(this.filters.filters[key]["filters_id"], f);
        this.$set(this.dataF, this.filters.filters[key]["filters_id"], f);
      }
      this.resultData = {};
    },
    clearFilters(){
      if(this.$route.name.match('catalog-brands')){
            this.dataF.category_id = []
            this.dataF.collection = []
      } else if(this.$route.name.match('catalog-collections')){
            this.dataF.category_id = [] 
      } else {
            this.dataF = {
            brand: [],
            category_id: [],
            collection: []
          };
      }

      this.dataPrice = [];
      for (const key in this.filters.filters) {
        this.$set(this.dataF, this.filters.filters[key]["filters_id"], []);
      }
      this.resultData = {};
    },
    onUpdateData() {
      if(this.resultData.count > 0){
        let r = {};
        for (const i in this.dataF) {
          if (this.dataF[i].length > 0 && i !== "brand" && i !== "category_id" && i !== "collection") {
            r[i] = this.dataF[i];
          }
        }
  
        // console.log(r);
        // console.log(this.dataF.brand);
        this.$emit('input', {
          f: r,
          price: this.dataPrice,
          brand: this.dataF.brand.length !== 0 ? this.dataF.brand : [],
          category_id: this.dataF.category_id.length !== 0 ? this.dataF.category_id : [],
          collection: this.dataF.collection.length !== 0 ? this.dataF.collection : []
        });
  
  
        this.dy = 0;
        window.scrollTo(0, 0);
      }
    },
    locationResult: debounce(async function(v){
      try {
        // console.log(v);
        // let rect = v.getBoundingClientRect();
        // console.log(rect);
        // let scrolltop = document.body.querySelector('.parent').getBoundingClientRect().top + scrollY;
        // console.log(scrolltop);
        // let scrolltop = window.scrollY + rect.top;
        // const middleY = scrolltop + rect.height / 2;
        // this.dy = middleY - this.disTop;
        // console.log(v)
        let r = {};
        for (const i in this.dataF) {
          if (this.dataF[i].length > 0 && i !== "brand" && i !== "category_id" && i !== "collection") {
            r[i] = this.dataF[i];
          }
        }
        
        let filtersCount = {
          category_id: this.$route.params.id ? this.$route.params.id : '',
          brand: this.dataF.brand.length !== 0 ? this.dataF.brand : {},
          collection: this.dataF.collection.length !== 0 ? this.dataF.collection : {},
          // category_id: this.dataF.category_id.length !== 0 ? this.dataF.category_id : {},
          price: this.dataPrice.length !== 0 ? this.dataPrice : {}
        }
        // console.log(filtersCount)
        let res, resPromote;
        if (this.$route.name.match('catalog')) {
          this.loading = true;
          try {
            res = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/filters/count', {
              params: {
                q: this.$route.query.q ? this.$route.query.q : '',
                f: r,
                filters: filtersCount,
              }
            });
            this.updateFiltersData(res.data.data);
          } catch (error) {
            console.error(error);
          }
          
          this.loading = false;
        }


        if (this.$route.name.match('promote')) {
          this.loading = true;
          try {
            resPromote = await this.$axios.get(this.$config.baseURL + '/api/site/promote_catalog/filters', {
              params: {
                f: r,
                filters: {
                  "ic.promote_id": this.$route.params.id,
                  brand: this.dataF.brand.length !== 0 ? this.dataF.brand : {},
                  collection: this.dataF.collection.length !== 0 ? this.dataF.collection : {},
                  price: this.dataPrice.length !== 0 ? this.dataPrice : {},
                  category_id: this.dataF.category_id.length !== 0 ? this.dataF.category_id : {}
                },
              }
            });
            this.updateFiltersData(resPromote.data.data);
          } catch (error) {
            console.error(error);
          }
          this.loading = false;
        }
        this.resultData = res ? res.data.data : resPromote.data.data
      } catch (error) {
        console.error(error)
      }
    }, 0),
    updateFiltersData(data) {
      const updatedData = data.filters;
      // console.log(data);
      // console.log(this.filters);
      // console.log(data);
      for (const el of this.filters.filters) {
        if (el.type == 1) {
          const updatedFilter = updatedData.find(e => e.filters_id == el.filters_id);
          if (updatedFilter) {
            for (const filterEl of el.filters_data) {
              // console.log(filterEl.value)
              if (updatedFilter.filters_data.find(e => e.value == filterEl.value)) {
                filterEl.disabled = false;
              } else {
                let r;
                // console.log(el.filters_id)
                for (const key in this.dataF) {
                  // console.log(el.filters_id)
                  if (this.dataF[key] && this.dataF[key].length > 0 && +key !== +el.filters_id) {
                    if(this.dataF.hasOwnProperty(el.filters_id) && this.dataF[el.filters_id].find(item => item == filterEl.value)){
                      r = false
                    } else {
                      r = true
                      break;
                    }
                    // console.log(+key == +el.filters_id ? [key, el.filters_id] : false)
                    // console.log(this.dataF[key]);
                  } else {
                    if(Object.values(this.dataF).find(item => item.length > 0)){
                  r = false
                } else {             
                  r = true
                }
                  }
                  // console.log(r);
                }
                r ? filterEl.disabled = true : filterEl.disabled = false
              }
            }
          } else {
            for (const filterEl of el.filters_data) {
                let r;
                for (const key in this.dataF) {
                  if (this.dataF[key] && this.dataF[key].length > 0 && +key !== +el.filters_id) {
                    if(this.dataF.hasOwnProperty(el.filters_id) && this.dataF[el.filters_id].find(item => item == filterEl.value)){
                      r = false
                    } else {
                      r = true
                      break;
                    }
                    // console.log(this.dataF[key]);

                  } else {
                    if(Object.values(this.dataF).find(item => item.length > 0)){
                      r = false
                    } else {             
                      r = true
                    }
                  }
                  // console.log(r);
                }
                // console.log('"Элемента ваще нигде нет"' + filterEl);
                r ? filterEl.disabled = true : filterEl.disabled = false
            }
          }
        }
      }

      for (const el of this.filters.brands) {
        const a = data.brands.find(e => e.brand == el.brand);
        if (a) {
          el.disabled = false;
        } else {
          // console.log(this.dataF.brand.find(item => item == el.brand));
          // console.log(this.dataF.brand);
          if (this.dataF.brand.find(item => item == el.brand)) {
            // console.log(el.brand)
            el.disabled = false;
          } else {
            let r;
            for (const key in this.dataF) {
              if (this.dataF[key] && this.dataF[key].length > 0 && key !== "brand") {
                if(this.dataF.brand.find(item => item == el.brand)){
                      r = false
                    } else {
                      r = true
                      break;
                    }
                // console.log(this.dataF[key]);

              } else {
                if(Object.values(this.dataF).find(item => item.length > 0)){
                  r = false
                } else {             
                  r = true
                }
              }
              // console.log(r);
            }
            r ? el.disabled = true : el.disabled = false
          }
        }
      }

      for (const el of this.filters.collections) {
        const b = data.collections.find(e => e.collection == el.collection);
        if (b) {
          el.disabled = false;
        } else {
          if (this.dataF.collection.find(item => item == el.collection)) {
            el.disabled = false;
          } else {
            let r;
            for (const key in this.dataF) {
              if (this.dataF[key] && this.dataF[key].length > 0 && key !== "collection") {
                if(this.dataF.collection.find(item => item == el.collection)){
                      r = false
                    } else {
                      r = true
                      break;
                    }
                // console.log(this.dataF[key]);

              } else {
                if(Object.values(this.dataF).find(item => item.length > 0)){
                  r = false
                } else {             
                  r = true
                }
              }
              // console.log(r);
            }
            r ? el.disabled = true : el.disabled = false
          }
        }
      }
    }
  }
};
</script>

<style lang="scss">
  .s-clear-filter{
    background-color: #e0e0e0 !important;
    border: unset !important;
  }
  .a-catalog-check1{
    .v-input--selection-controls__ripple{
      display: none !important;
    }
  }
</style>