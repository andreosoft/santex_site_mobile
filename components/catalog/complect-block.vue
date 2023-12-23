<template>
  <div style="border: 1px solid #ddd" class="parent-complect">
    <div v-if="data" class="d-flex pa-8" style="">
      <div v-for="(el, i) in data" :key="i" class="">
        <div class="d-flex align-center">
          <div class="pa-8" v-if="i != 0" style="font-size: 30px">
            <i class="far fa-plus"></i>
          </div>
          <div class="black--text" style="width: 180px">
            <v-checkbox />
            <div>
              <v-img :src="el.image[0]" />
            </div>
            <div>{{ el.name }}</div>
            <div class="mt-4">
              <span style="font-size: 22px">
                <number :value="el.price" /> ₽
              </span>
              <span class="ml-4 grey--text " style="
              display: inline-block;
                    padding-top: 2px;
                      font-size: 16px;
                      vertical-align: top;
                      text-decoration: line-through;
                    " v-if="el.old_price">
                <number :value="el.old_price" /> ₽
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="d-flex pa-8 justify-space-between align-center">
      <div class="d-flex align-sm-center flex-column flex-sm-row parent-complect-button">
        <div class="mr-8 order-last order-sm-first s-button-complete">
          <v-btn dark class="s-btn-main"><b>В корзину комплектом</b></v-btn>
        </div>
        <div class="mb-5 mb-sm-0">
          <div class="s-price-complete">
            <span style="font-size: 32px; font-weight: bold;">
              <number :value="27345" /> <span style="font-weight: normal;">₽</span>
            </span>
            <span class="ml-4 grey--text " style="
                    display: inline-block;
                    padding-top: 6px;
                      font-size: 16px;
                      vertical-align: top;
                      text-decoration: line-through;
                    " v-if="true">
              <number :value="18345" /> ₽
            </span>
          </div>
          <div class="s-complect">Комплект: <b>2 товара</b></div>
        </div>
        <div class="s-items-complete d-block d-sm-none mb-4">
          <span class="grey--text" style="cursor: pointer">
            <span>Еще товары 1</span>
            <i style="font-size: 12px;" class="ml-4 fas fa-chevron-down"></i>
          </span>
        </div>
      </div>
      <div class="s-items-complete d-none d-sm-block">
        <span class="grey--text" style="cursor: pointer">
          <span>Еще товары 1</span>
          <i style="font-size: 12px;" class="ml-4 fas fa-chevron-down"></i>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: Array
  }
}
</script>

<style lang="scss">

@media screen and (max-width: 830px) {
  .s-items-complete{
    span{
      font-size: 12px !important;
    }
  }
  .s-complect{
    font-size: 12px !important;
  }
  .s-price-complete>span:first-child{
      font-size: 28px !important;
    }
  
  .s-button-complete{
    .s-btn-main{
      font-size: 12px !important;
    }
  }
}
@media screen and (max-width: 750px) {
  .s-items-complete{
    span{
      font-size: 11px !important;
    }
  }
  .s-complect{
    font-size: 11px !important;
  }
  .s-price-complete>span:first-child{
      font-size: 26px !important;
    }
  
  .s-button-complete{
    .s-btn-main{
      width: 230px;
      font-size: 11px !important;
    }
  }
}
@media screen and (max-width: 660px) {
  .s-button-complete{
    .s-btn-main{
      width: 200px;
      font-size: 10px !important;
    }
  }
  .s-price-complete>span:first-child{
    font-size: 22px !important;
  }
  .s-price-complete>span:not(:first-child){
    font-size: 12px !important;
  }
}
@media screen and (max-width: 600px) {
  .parent-complect{
    border: unset !important;
  }
}
@media screen and (max-width: 599px) {
  .parent-complect-button{
    width: 100% !important;
  }
  .s-items-complete{
    span{
      font-size: 12px !important;
    }
  }
  .s-complect{
    font-size: 12px !important;
  }
  .s-price-complete>span:first-child{
      font-size: 28px !important;
    }
  
    .s-button-complete{
    width: 100% !important;
    .s-btn-main{
      width: 100% !important;
      font-size: 12px !important;
    }
  }
}
</style>