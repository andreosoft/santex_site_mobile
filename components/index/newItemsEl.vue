<template>
  <v-card>
    <NuxtLink :to="'/promote/' + el.id">
      <img class="w-100 h-100" v-if="el.images_mobile && el.images_mobile[0]"
        :src="$config.baseImageURL + el.images_mobile[0] + '?height=500'" />
      <img class="w-100 h-100" v-else-if="el.images && el.images[0]" :src="$config.baseImageURL + el.images[0]" />
      <div class="s-cart-tite-1">
        <div class="pa-10 d-flex">
          <div class="d-inline-block mr-3" style="vertical-align: top">
            <v-btn fab><img src="/icons/basket.svg" alt="" /></v-btn>
          </div>
          <div class="d-inline-block white--text">
            <div class="text-uppercase text-h5">
              <b>{{ el.collection ? el.collection : el.introtext ? el.introtext : 'Не указано' }}</b>
            </div>
            <div><b>{{ el.name ? el.name : 'Не указано' }}</b> <i class="fa fa-long-arrow-right"></i></div>
          </div>
        </div>
      </div>
    </NuxtLink>
  </v-card>
</template>

<script>
export default {
  props: {
    el: Object,
    sales: {
      type: Boolean,
      default: false
    }
  }
};
</script>