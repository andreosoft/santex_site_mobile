<template>
    <v-card id="s-card-good-small" class="s-card-good py-5">
      <div>
        <v-col class="pa-0 col-12 d-flex flex-row justify-space-between">
          <v-col class="mb-2 pa-0 pr-4 d-flex flex-column align-start col-4" style="position: relative;">
            <div style="position: relative;" class="mb-2">
                <v-img class="s-item-list-img" :contain="true" style="width: 150px; height: 150px; border: 1px solid rgb(192, 192, 192)" :src="$config.baseImageURL+el.images+'?height=250'" />
                <div style="position: absolute; right: 0px; top: 0px">
                  <v-btn @click="removeItem(el)" icon style="padding-bottom: 4px;"><i style="font-size: 20px;" class="grey--text fas fa-times-circle"></i></v-btn>
                </div>
              </div>
            <div class="d-flex flex-column align-start mt-1 s-item-list-icons">
              <v-btn @click="toCompare(el)" class="pa-0 w-100 d-flex justify-start">
                <img src="/icon-similar.png" alt="" />
                <p class="ma-0 pa-0 ms-2">Сравнить</p>
              </v-btn>
            </div>
          </v-col>
          <v-col class="pa-0">
          
          <div class="d-flex flex-column" style="height: 100%;">
                <nuxt-link :to="'/catalog/view/' + el.id">
              <div class="d-flex flex-row justify-space-between mb-2">
                <div class="s-item-list-available" style="margin: 3px 0; font-size: 13px">
                  <catalog-available :value="el.store" />
                </div>
                <div class="s-item-list-id" style="margin: 3px 0; font-size: 13px; color: #949494">
                  Код товара: {{ el.id }}
                </div>
              </div>
              <div class="mb-4 s-item-list-name" style="margin: 3px 0; font-size: 16px; font-weight: bold" :class="{ 'hidden-text': hiddentext }">{{ el.name }}</div>
              <div class="my-1 s-item-list-size" style="font-size: 11px">
                <div>
                  <!-- <span style="color: #949494">Габариты (Д.Ш.В): </span><span>{{ el.width }}</span> -->
                  <!-- <div v-if="itemList.depth && itemList.height"><span style="color: #949494">Габариты (Г.Ш.В): </span>
                    <span>{{`${itemList.depth + ' x '} ${itemList.width} ${' x ' + itemList.height}` }}</span>
                  </div>
                  <div v-else-if="!itemList.height"><span style="color: #949494">Габариты (Г.Ш): </span>
                    <span>{{`${itemList.depth + ' x '} ${itemList.width}`}}</span>
                  </div> -->
                  <div v-if="el.height && el.width && el.length"><span style="color: #949494">Габариты (Д.Ш.В): </span>
                    <span>{{`${el.length} x ${el.width} x ${el.height}` }}</span>
                  </div>
                  <div v-else-if="!el.length && el.width && el.height"><span style="color: #949494">Габариты (Ш.В): </span>
                    <span>{{`${el.width} ${' x ' + el.height}` }}</span>
                  </div>
                  <div v-else-if="!el.width && el.length && el.height"><span style="color: #949494">Габариты (Д.В): </span>
                    <span>{{`${el.length}${' x ' + el.height}` }}</span>
                  </div>
                  <div v-else-if="!el.height && el.length && el.width"><span style="color: #949494">Габариты (Д.Ш): </span>
                    <span>{{`${el.length} x ${el.width}` }}</span>
                  </div>
                  <div v-else><span style="color: #949494">Габариты (Д.Ш.В): </span>
                    <span>Не указаны</span>
                  </div>
                    
                    
                    
                </div>
                <div class="s-item-list-brand">
                  <span  style="color: #949494">Бренд: </span><span>{{ el.brand }}</span>
                </div>
              </div>
              <div class="wrapper"></div>
            </nuxt-link>
              <div class="my-2 my-sm-0 d-flex flex-row justify-space-between align-center s-item-list-price" style=" font-weight: bold;">
                <div class="d-flex flex-column align-end">
                  <span v-if="el.price" class="s-item-list-price-main" style="font-size: 20px">
                    <number :value="el.price" /> ₽
                  </span>
                  <span v-else class="s-item-list-price-second" style="font-size: 20px">
                    Цена не указана
                  </span>
                  <span class="mx-2" v-if="el.price_old" style="font-size: 13px; text-decoration: line-through; color: #949494">
                    <number :value="el.price_old" /> ₽
                  </span>
                </div>
                <v-btn @click="toCart(el)" dark class="s-btn-cart s-btn-text">В корзину</v-btn>
              </div>
            </div>
            </v-col>
          </v-col>
      </div>
    </v-card>
  </template>
  
  <script>
  export default {
    props: {
      el: Object,
      hiddentext: {
      type: Boolean,
      default: false
    }
    },
    data()  {
      return {
      }
    },
    methods: {
      removeItem(el){
        this.$store.commit('favorite/remove', el);
        this.$emit('removeItemFav', true);
      },
      async toCompare(el){
        try{
          let respCom = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/' + el.id);
          const dataCom = respCom.data.data.filters;
          // console.log(dataCom);
          let item = {
                    id: el.id,
                    category_id: respCom.data.data.category_id,
                    name: el.name,
                    image: el.images,
                    price: el.price,
                    old_price: el.price_old,
                    brand: el.brand,
                    available: 1,
                    dataParams: {}
                  };
                  dataCom.forEach((element, index) => {
                    if(index!==0){
                      if(element.name === 'Прозводитель'){
                        element.name = 'Бренд';
                      }
                      item.dataParams[element.name] = element.value
                    }
                  });
                  // console.log(item)
                  this.$store.commit('compare/addItem', item);
                  this.$emit('addItemCom', true);
      } catch(e){
        console.log(e);
      }},
      async toCart(el){
        try{
          let respCom = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/' + el.id);
          const dataCom = respCom.data.data.filters;
          // console.log(dataCom);
          let height = '';
          let width = '';
          let depth = '';
          let lengthItem = '';
          dataCom.forEach(item => {
            if(item.name === 'Высота'){
              height = item.value
            } else if(item.name === 'Ширина') {
              width = item.value
            } else if(item.name === 'Глубина'){
              depth = item.value
            } else if(item.name === 'Длина'){
              lengthItem = item.value;
            }
          });
          // console.log('Высота ' + height)
          // console.log('Ширина ' + width)
          // console.log('Глубина ' + depth)
          // console.log('Длина ' + lengthItem)
          const item = {
            code: el.id,
            name: el.name,
            img: el.images,
            price: el.price,
            old_price: el.price_old,
            brand: el.brand,
            count: 1,
            type: el.type,
            width,
            height,
            depth,
            lengthItem,
          }
          this.$store.commit('cart/add', item);
          this.$emit('addItemCart', true);
        } catch (e){
          console.error(e);
        }
    }
    }
  };
  </script>
  
  
  <style lang="scss" scoped>
  .hidden-text{
    height: 48px;
    position: relative;
    hyphens: auto;
    overflow: hidden !important;
  }
  .hidden-text:after{
    content: "";
    text-align: right;
    position: absolute;
    bottom: 0;
    right: 0;
    width: 70%;
    height: 1.2em;
    background: linear-gradient(to right, rgba(255, 255, 255, 0), white 100%);
    pointer-events: none;
  }
  
  
  @media screen and (max-width: 960px) {
    .s-item-list-btn{
      .s-btn-cart{
        width: 50%;
        font-size: 10px !important;
        min-width: unset !important;
      }
    }
  }
  @media screen and (max-width: 768px) {
    .s-item-list-available{
      span{
        font-size: 12px !important;
      }
    }
    .s-item-list-id{
      font-size: 12px !important;
    }
    .s-item-list-name{
      font-size: 14px !important;
    }
    .s-item-list-size{
      span{
        font-size: 10px !important;
      }
    }
    .s-item-list-price{
      span{ 
        font-size: 18px !important;
      }
    }
    .s-item-list-icons{
      img{
        width: 1.4em !important;
      }
    }
  }
  @media screen and (max-width: 668px) {
    
  }
  
  </style>