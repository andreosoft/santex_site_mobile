<template>
    <div>
        <div class="s-text-h2 text-center mb-10">Распродажа</div>
        <div class="mb-sm-14">
          <div class="d-none d-lg-block">
            <v-row>
              <v-col class="pa-0" lg="6" v-for="(el, i) in activeData" :key="i">
                <index-new-items-el :sales="true" style="height: 488px" :el="el" />
              </v-col>
            </v-row>
          </div>
          <common-carusel class="d-block d-lg-none" :items="items" :slides="1" :sales="true"/>
          <!-- <div class="d-none d-sm-block d-lg-none">
            <v-carousel v-model="carouselModel">
              <v-carousel-item v-for="(el, i) in items" :key="i">
                <index-new-items-el-carusel :el="el" />
              </v-carousel-item>
            </v-carousel>
          </div>
          <div class="d-block d-sm-none">
            <v-carousel v-model="carouselModel">
              <v-carousel-item v-for="(el, i) in items" :key="i">
                <index-new-items-el-carusel :el="el" />
              </v-carousel-item>
            </v-carousel>
          </div> -->
        </div>
        <v-container>
          <div class="text-center mt-sm-14">
            <v-btn @click="toggleTwo < items.length ? toggleTwo+=2 : toggleTwo=2" class="s-btn-main s-btn-text">
              <NuxtLink to="#">БОЛЬШЕ ТОВАРОВ</NuxtLink>
            </v-btn>
          </div>
        </v-container>
      </div>
</template>

<script>

export default {
    props: {
        items: Array
    },
    data() {
        return {
          carouselModel: 0,
          toggleTwo: 2,
        }
    },
    computed: {
    activeData() {
      return this.items.slice(0, this.toggleTwo); 
    },
  }
}
</script>