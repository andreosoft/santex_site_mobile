<template>
  <div class="parent">
    <!-- <p>filter {{ value }}</p> -->
    <!-- <div>
      <v-btn @click="onUpdateData()">Показать</v-btn>
    </div> -->
    <v-row id="filter-head" class="d-flex flex-row align-center justify-end">
      <div @click="$emit('filter-small', $event)" class="d-flex align-center justify-center" style="cursor: pointer">
        <i style="width: 100%" class="fa fa-times"></i>
      </div>
    </v-row>
    <h3 class="pa-0 ma-0 mb-8 "><b>Фильтр</b></h3>
    <catalog-filterResult class="d-none d-md-block" @filterResult="onUpdateData" :locationRes="dy" :resultData="resultData"/>
    <div class="space-check">
      <catalog-price @location="locationResult" title="Цена, руб." v-model="dataPrice" :max="filters.price.max_price" :min="filters.price.min_price" />
        <catalog-brands
        v-if="filters.brands"
        @location="locationResult"
        :params="filters.brands"
        v-model="dataF.brand"
        />
        <catalog-categories class="mt-4"
        v-if="filters.categories"
        @location="locationResult"
        :params="filters.categories"
        v-model="dataF.category_id"
        />
      <v-divider class="my-4" />
    </div>
    <div v-for="(el, i) in filters.filters" :key="i">
      <div>
        <div v-if="el.type == 2" class="space-check">
          <catalog-ranges v-if="el.numFilters.length > 1" @location="locationResult" :title="el.name" v-model="dataF[el.filters_id]" :params="el.numFilters" :min="el.min" :max="el.max" />
        </div>
        <div v-else class="space-check">
          <catalog-check1 v-if="el.filters.length > 1" @location="locationResult" :title="el.name" v-model="dataF[el.filters_id]" :params="el.filters" />
          <v-divider v-if="el.filters.length > 1" class="my-4" />
        </div>
      </div>
    </div>
    <v-btn @click="onUpdateData" dark class="s-btn-cart s-btn-text w-100 d-flex d-md-none">       
     Показать
     <number v-if="resultData > 0" class="ml-2" :value="resultData" />
  </v-btn>
    <v-btn @click="clearFilters" class="s-btn-cart s-btn-text s-clear-filter w-100 mt-3 d-flex d-md-none">       
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
    }
  },
  data() {
    return {
      dataF: {
        brand: [],
        category_id: [],
      },
      dataPrice: [],
      dy: 0,
      resultData: 0,
      disTop: 0
    };
  },
  created() {
    this.initValueFilters();
  },
  mounted() {
    // console.log(this.value);
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
        // console.log("Инициализировано")
      // }
      // catch (error){
      //   console.error(error)
      // }
    }
  },
  computed: {
    // onUpdateData() {
    //   if(this.resultFilterValue){
    //     let r = {};
    //     for (const i in this.dataF) {
    //       if (this.dataF[i].length > 0) {
    //         r[i] = this.dataF[i];
    //       }
    //     }
    //     this.$emit('input', { f: r, price: this.dataPrice });
    //   }
    // },
    // locationRes(){
    //     document.body.querySelector('.s-filterResult').style.left = this.dx + 'px'
    //     document.body.querySelector('.s-filterResult').style.top = this.dy + 'px'
    // }
  },
  methods: {
    initValueFilters() {
      this.dataF = {
        brand: this.value.brand ? this.value.brand : [],
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
      this.resultData = 0;
        // for (const key in this.filters.brands) {
        //   if(this.filters.brands[key].brand){
        //     let f = [];
        //     if (this.value?.f[this.filters.brands[key].brand]) f = this.value.f[this.filters.brands[key].brand];
        //     this.$set(this.dataF["brands"], this.filters.brands[key].brand, f);
        //   }
        // }
    },
    clearFilters(){
      this.dataF = {
        brand: [],
        category_id: [],
      };
      this.dataPrice = [];
      for (const key in this.filters.filters) {
        // let f = [];
        // if (this.value?.f[this.filters.filters[key]["filters_id"]]){
        //   f = this.value.f[this.filters.filters[key]["filters_id"]];
        // } 
        // console.log(this.filters.filters[key]["filters_id"], f);
        this.$set(this.dataF, this.filters.filters[key]["filters_id"], []);
      }
      this.resultData = 0;
    },
    onUpdateData() {
      let r = {};
      for (const i in this.dataF) {
        if (this.dataF[i].length > 0 && i !== "brand" && i !== "category_id") {
          r[i] = this.dataF[i];
        }
      }

      // console.log(r);
      // console.log(this.dataF.brand);
      this.$emit('input', { f: r, price: this.dataPrice, brand: this.dataF.brand.length !== 0 ? this.dataF.brand : [], category_id: this.dataF.category_id.length !== 0 ? this.dataF.category_id : []  });

      this.dy = 0;
      window.scrollTo(0, 0);
    },
    locationResult: debounce(async function(v){
      try {
        let w = Object.values(this.dataF).find(item => {return item.length>0});
        if(!w){
          this.resultData = 0;
        } else {
        // console.log(v);
        let rect = v.getBoundingClientRect();
        // console.log(rect);
        // let scrolltop = document.body.querySelector('.parent').getBoundingClientRect().top + scrollY;
        // console.log(scrolltop);
        let scrolltop = window.scrollY + rect.top;
        const middleY = scrolltop + rect.height / 2;
        this.dy = middleY - this.disTop;

        let r = {};
        for (const i in this.dataF) {
          if (this.dataF[i].length > 0 && i !== "brand" && i !== "category_id") {
            r[i] = this.dataF[i];
          }
        }
        let filtersCount = {
          category_id: this.$route.params.id ? this.$route.params.id : '',
          brand: this.dataF.brand.length !== 0 ? this.dataF.brand : {},
          price: this.dataPrice.length !== 0 ? this.dataPrice : {},
          status: 1
        }

        if (this.$route.query.q) Object.assign(filtersCount, { "OR": [
            {id: { condition: "LIKE", value: "%" + this.$route.query.q + "%" }},
            {name: { condition: "LIKE", value: "%" + this.$route.query.q + "%" }},
            {vendor: { condition: "LIKE", value: "%" + this.$route.query.q + "%" }},
            {factory_article: { condition: "LIKE", value: "%" + this.$route.query.q + "%" }}] });

            let res, resPromote;
            if(this.$route.name.match('catalog')){
              res = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/count', { 
          params: {
            f: r, 
            filters: filtersCount,
            // sort: {
            //   price: 'asc',
            //   order: 'asc'
            // },
            // pager: {
            //   count: 0,
            //   limit: 30,
            //   page: "0"
            // },
          }
        });
            }
        if(this.$route.name.match('promote')){
          resPromote = await this.$axios.get(this.$config.baseURL + '/api/site/promote_catalog/count', { 
            params: {
              f: r, 
              filters: {
                "ic.promote_id": this.$route.params.id,
                brand: this.dataF.brand.length !== 0 ? this.dataF.brand : {},
                price: this.dataPrice.length !== 0 ? this.dataPrice : {},
                category_id: this.dataF.category_id.length !== 0 ? this.dataF.category_id : {},
                status: 1
              },
              // sort: {
              //   price: 'asc',
              //   order: 'asc'
              // },
              // pager: {
              //   count: 0,
              //   limit: 30,
              //   page: "0"
              // },
            }
          });
        }
        this.resultData = res ? res.data.data : resPromote.data.data
      }} catch (error) {
        console.error(error)
      }
    }, 1000)
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