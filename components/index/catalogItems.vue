<template>
  <div>
    <div id="head" class="s-text-h2 text-center mb-10">Каталог товаров</div>
    <div class="d-none d-xl-block">
      <v-row>
        <v-col class="pa-0" xl="2"  v-for="(el, i) in items.slice(0,12)" :key="i">
          <index-catalog-items-el :el="el" />
        </v-col>
      </v-row>
    </div>
    <div class="d-none d-lg-block">
      <v-row>
        <v-col class="pa-0" lg="3"  v-for="(el, i) in items.slice(0,8)" :key="i">
          <index-catalog-items-el :el="el" />
        </v-col>
      </v-row>
    </div>
    <div class="d-none d-sm-block d-lg-none">
      <v-row>
        <v-col class="pa-0" cols="4" v-for="(el, i) in dataCatalogSM" :key="i">
          <index-catalog-items-el :el="el" />
        </v-col>
      </v-row>
    </div>
    <v-divider/>
    <div class="d-block d-sm-none mt-5">
      <v-row v-for="(el, i) in dataCatalogXS" :key="i">
        <NuxtLink :to="'/catalog/' + el.id" style="width: 100%">
          <div class="d-flex flex-row align-center">
            <div style="width: 50px; height: 50px; margin: 10px 20px">
              <v-img v-if="el.images" :src="$config.baseImageURL + el.images[0]" />
              <v-img v-else src="/black-square.jpg" />
            </div>
            <!-- <div style="background-color: black; "></div> -->
            <p style="margin-bottom: 0;">{{ el.name }}</p>
            <svg style="margin-left: auto; margin-right: 10px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="8" height="14" viewBox="0 0 8 14">
              <defs>
                <clipPath id="clip-path">
                  <rect id="Rectangle_31" data-name="Rectangle 31" width="14" height="8"/>
                </clipPath>
              </defs>
              <g id="arrow_right" transform="translate(8) rotate(90)">
                <g id="Group_176" data-name="Group 176" clip-path="url(#clip-path)">
                  <path id="Path_141" data-name="Path 141" d="M7.169,0A.989.989,0,0,0,6.51.274L.3,6.276A1.008,1.008,0,0,0,.281,7.695a.991.991,0,0,0,1.4.027L7.2,2.384l5.516,5.338a.99.99,0,0,0,1.408-.032A1.008,1.008,0,0,0,14.1,6.276L7.89.274A.989.989,0,0,0,7.169,0" transform="translate(0 0)"/>
                </g>
              </g>
            </svg>
          </div>
          <v-divider/>
        </NuxtLink>
      </v-row>
    </div>
    <v-container>
      <div class="text-center mt-sm-14 pt-8 all-cat" style="width: 35%; margin: 0 auto">
        <v-btn style="width: 100% !important" @click="AllCat" class="s-btn-main s-btn-text">ВСЕ КАТЕГОРИИ</v-btn>
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
            dataCatalogXS: this.items.slice(0, 10),
            dataCatalogSM: this.items.slice(0, 9),
        }
    },
    methods: {
        AllCat() {
          if(this.dataCatalogSM.length < this.items.length){
            this.dataCatalogSM = this.items
          } else{
            this.dataCatalogSM = this.items.slice(0, 9)
          }
          if(this.dataCatalogXS.length < this.items.length){
            this.dataCatalogXS = this.items
          } else{
            this.dataCatalogXS = this.items.slice(0, 10)
          }
        }
    }
}
</script>

<style lang="scss">
@media screen and (max-width: 600px) {
  .all-cat{
    width: 100% !important;
    margin: 0 auto !important;
  }
}
</style>