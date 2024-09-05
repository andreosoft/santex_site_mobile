<template>
    <v-container class="mb-10 s-promote-main">
        <v-divider class="mb-8" />
        <common-beadcrumbs class="mb-4" :value="breadcrumbsDataPromote" />
        <div class="w-100 s-static-main">
          <h1 v-if="infoPromote && infoPromote[0].name">{{ infoPromote[0].name }}</h1>
        </div>
        <v-divider class="mb-8" />
        <div class="d-flex flex">
          <img class="w-100" v-if="carouselItems && carouselItems.length" :src="$config.baseImageURL + carouselItems[carouselItems.length - 1]" alt="banner">
          <!-- <index-mainCarusel :items="carouselItems"/> -->
        </div>
        <v-divider v-if="carouselItems && carouselItems.length"  class="mb-8" />
        <h4 v-if="infoPromote && infoPromote[0].introtext" v-html="infoPromote[0].introtext"></h4>
        <div v-if="infoPromote && infoPromote[0].content" v-html="infoPromote[0].content"></div>
        <v-divider class="mb-8" />
        <catalog-base-catalog 
          v-if="dataPromote?.length > 0" 
          :data="dataPromote" 
          :loading="loading" 
          :dataFilters="dataFiltersPromote"
          :valueFilters="valueFiltersPromote" 
          :activeFilters="activeFilters"
          :pager="pagerPromote" 
          :sort="sort" 
          @update-data="valueFiltersPromote = $event"
        />
    <div class="text-center mt-10">
      <!-- <common-pagination :value="pager" /> -->
    </div>
      </v-container>
</template>

<script>

import {getDataPromote} from "@/pages/promote/getDataPromote";
export default {
  data() {
    return { 
      loading: true
    }
  },
  async asyncData({ route, $axios, $config, error }) {
    return await getDataPromote({ route, $axios, $config, error });
  },
  watch: {
    valueFiltersPromote(v) {
      let filters = {};
      if (v.price && v.price.length > 0) {
        filters.price = v.price;
      }
      if (v.brand && v.brand.length > 0) {
        filters.brand = v.brand;
      }
      if (v.collection && v.collection.length > 0) {
        filters.collection = v.collection;
      }
      if (v.category_id && v.category_id.length > 0) {
        filters.category_id = v.category_id;
      }
      this.$router.push({ query: Object.assign({}, this.$route.query, { filters: JSON.stringify(filters), f: JSON.stringify(v.f), page: 0 }) });
    },
    "$route": {
      async handler() {
        this.loading = true;
        let p = await getDataPromote({ route: this.$route, $axios: this.$axios, $config: this.$config, error: this.$error });
        this.loading = false;
        this.activeFilters = p.activeFilters;
        this.dataPromote = p.dataPromote;
        this.pagerPromote = p.pagerPromote;
      },
    }
  },
}
</script>

<style lang="scss">
@media screen and (max-width: 600px) {
  .s-promote-main{
    h2{
      font-size: 22px;
    }
    p{
      font-size: 16px;
    }
  }
}
@media screen and (max-width: 425px) {
  .s-promote-main{
    p{
      font-size: 14px;
    }
  }
}
</style>