<template>
  <v-card>
    <NuxtLink :to="el.to ? el.to : '/promote/' + el.id">
      <v-img style="width: 100% !important" :src="el.img ? el.img : $config.baseImageURL + el.images[el.images.length-1]" />
      <div class="s-cart-tite-1" :class="{ 'text-info-sales': sales }">
        <div class="pa-10">
          <div class="d-inline-block mr-2 mr-sm-3" style="vertical-align: top">
            <v-btn fab><img src="/icons/basket.svg" alt="" /></v-btn>
          </div>
          <div class="d-inline-block white--text text-info">
            <div class="text-uppercase">
              <b>{{ el.titleCollection ? el.titleCollection : el.introtext }}</b>
            </div>
            <div><b>{{ el.titleItem ? el.titleItem : el.name }}</b> <i class="fa fa-long-arrow-right"></i></div>
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
  },
};
</script>

<style lang="scss">

  .v-carousel__controls{
    background-color: unset;
  }
  @media screen and (max-width: 600px) {
    .s-cart-tite-1{
      margin-left: -10px;
      margin-bottom: -25px;
      div>div:first-child button{
        width: 45px;
        height: 45px;
        padding: 10px;
        img{
          width: 20px;
        }
      }
      .text-info div:nth-child(odd){
        font-size: 20px !important;
      }
      .text-info div:nth-child(even){
        font-size: 14px !important;
      }
    }
  }
  @media screen and (max-width: 480px) {
    .s-cart-tite-1{
      
      div>div:first-child button{
        width: 35px;
        height: 35px;
        padding: 5px;
        img{
          width: 16px;
        }
      }
      .text-info div:nth-child(odd){
        font-size: 16px !important;
      }
      .text-info div:nth-child(even){
        font-size: 10px !important;
      }
    }
    .text-info-sales{
      margin-left: -10px;
      margin-bottom: -35px;
    }
  }
</style>