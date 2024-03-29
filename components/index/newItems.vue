<template>
  <div>
    <div style="width: 100%" class="s-text-h2 text-center mb-5">Новинки: из коллекций лучших брендов</div>
    <div class="mb-sm-14" style="width: 100%">
      <div class="d-none" id="new-items">
        <v-row>
          <v-col class="pa-0 col-12" v-for="(el, i) in items" :key="i">
            <index-new-items-el :el="el" />
          </v-col>
        </v-row>
      </div>
      <div class="d-block" id="new-items-slider">
        <common-carusel :newItems="true" :sales="true" :items="items" :slides="1" :pager="true"/>
      </div>
    </div>
    <!-- <v-container>
      <div class="text-center mt-sm-14">
        <v-btn class="s-btn-main s-btn-text">
          <NuxtLink to='#'>БОЛЬШЕ НОВИНОК</NuxtLink>
        </v-btn>
      </div>
    </v-container> -->
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
  },
  data() {
    return {
      carouselModel: 0,
      toggleTwo: 2,
    };
  },
  computed: {
    activeData() {
      return this.items.slice(0, this.toggleTwo); 
    },
  }
};
</script>

<style lang="scss">
.s-cart-tite-1 {
  position: absolute;
  bottom: 0;
}
.v-btn > .v-btn__content .v-icon{
  color: black;
}

</style>