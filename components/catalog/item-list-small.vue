<template>
  <v-card id="s-card-good-small" class="s-card-good py-5">
    <div>
      <v-col class="pa-0 col-12 d-flex flex-row justify-space-between">
        <v-col class="mb-2 pa-0 pr-4 d-flex flex-column align-start col-4" style="position: relative;">
          <v-img @click="$router.push(routeItem)" class="s-item-list-img" v-if="el.images && el.images[0]" :contain="true" style="width: 150px; height: 150px; border: 1px solid rgb(192, 192, 192); cursor: pointer" :src="$config.baseImageURL+el.images[0]+'?height=250'" />
          <div class="d-flex flex-column align-start mt-1 s-item-list-icons" style="width: 150px;">
            <v-btn @click="toFavorite(el)" class="pa-0 w-100 d-flex justify-start">
              <img v-show="checkAvailable" src="/icons/like-black.svg" alt="favorite-black">
              <img v-show="!checkAvailable" src="/icons/like.svg" alt="favorite">
              <p class="ma-0 pa-0 ms-2">В избранное</p>
            </v-btn>
            <v-btn @click="toCompare(el)" class="pa-0 w-100 d-flex justify-start">
              <img src="/icon-similar.png" alt="" />
              <p class="ma-0 pa-0 ms-2">Сравнить</p>
            </v-btn>
          </div>
        </v-col>
        <v-col class="pa-0">
        
        <div class="d-flex flex-column" style="height: 100%;">
                <nuxt-link :to="'/catalog/view/' + el.id" class="d-flex flex-column" style="height: 100%;">
            <div class="d-flex flex-row justify-space-between mb-2">
              <div class="s-item-list-available" style="margin: 3px 0; font-size: 13px">
                <catalog-available :value="el.store" />
              </div>
              <div class="s-item-list-id" style="margin: 3px 0; font-size: 13px; color: #949494">
                Код товара: {{ el.id }}
              </div>
            </div>
            <div class="mb-4 s-item-list-name" style="margin: 3px 0; font-size: 16px; font-weight: bold; word-break: break-word" :class="{ 'hidden-text': hiddentext }">{{ el.name }}</div>
            <div class="my-1 s-item-list-size" style="font-size: 11px">
              <div>
                <!-- <span style="color: #949494">Габариты (Д.Ш.В): </span><span>{{ el.width }}</span> -->
                <!-- <div v-if="itemList.depth && itemList.height"><span style="color: #949494">Габариты (Г.Ш.В): </span>
                  <span>{{`${itemList.depth + ' x '} ${itemList.width} ${' x ' + itemList.height}` }}</span>
                </div>
                <div v-else-if="!itemList.height"><span style="color: #949494">Габариты (Г.Ш): </span>
                  <span>{{`${itemList.depth + ' x '} ${itemList.width}`}}</span>
                </div> -->
                <div v-if="el.hight && el.width && el.length"><span style="color: #949494">Габариты (Д.Ш.В): </span>
                  <span>{{`${el.length} x ${el.width} x ${el.hight}` }}</span>
                </div>
                <div v-else-if="!el.length && el.width && el.hight"><span style="color: #949494">Габариты (Ш.В): </span>
                  <span>{{`${el.width} ${' x ' + el.hight}` }}</span>
                </div>
                <div v-else-if="!el.width && el.length && el.hight"><span style="color: #949494">Габариты (Д.В): </span>
                  <span>{{`${el.length}${' x ' + el.hight}` }}</span>
                </div>
                <div v-else-if="!el.hight && el.length && el.width"><span style="color: #949494">Габариты (Д.Ш): </span>
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
import { mapGetters } from 'vuex'
export default {
  props: {
    el: Object,
    hiddentext: {
      type: Boolean,
      default: false
    }
  },
  data(){
    return {
      routeItem: '/catalog/view/' + this.el.id,
      itemList: {
        depth: '',
        width: '',
        height: '',
        lengthItem: '',
      }
      // item: {
      //   code: this.el.id,
      //   name: this.el.name,
      //   img: this.el.images[0] !== null ? this.el.images[0] : '',
      //   price: this.el.price,
      //   old_price: this.el.price_old,
      //   brand: this.el.brand,
      //   count: 1,
      //   type: this.el.type,
      // }
    }
  },
  computed: {
    ...mapGetters({
      dataFav: 'favorite/favItems'
    }),
    checkAvailable(){
      const sim = this.dataFav.find((item) => {if(item.id === this.el.id){ return item }});
        if (sim) {return true;} else {return false;}
    }
  },
  methods: {
    async toCompare(el){
      try{
        let respCom = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/' + el.id);
        const dataCom = respCom.data.data.filters;
        // console.log(dataCom);
        let item = {
          id: el.id,
          category_id: el.category_id,
          name: el.name,
          image: el.images ? el.images[0] : '',
          price: el.price,
          old_price: el.price_old,
          brand: el.brand,
          available: el.store,
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

        this.$store.commit('compare/addItem', item);
        this.$emit('addItemCom', true);
      } catch(e){
        console.error(e);
      }
      
    },
    async toFavorite(el){
      if(!this.checkAvailable){
      try{
        let respCom = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/' + el.id);
        const dataCom = respCom.data.data.filters;
        // console.log(dataCom);
        let depth = '';
        dataCom.forEach(item => {if(item.name === 'Глубина'){depth = item.value}});
          const item = {
            id: el.id,
            name: el.name,
            images: el.images[0],
            price: el.price,
            old_price: el.price_old,
            brand: el.brand,
            count: 1,
            type: el.type,
            store: el.store,
            width: el.width,
            height: el.hight,
            depth,
            lengthItem: el.length,
          }
          // console.log('Высота ' + itemFav.height)
          // console.log('Ширина ' + itemFav.width)
          // console.log('Глубина ' + itemFav.depth)
          // console.log('Длина ' + itemFav.lengthItem)
          // console.log(itemFav);
            // this.$router.push('/favorite');
          this.$store.commit('favorite/addItem', item);
          this.$emit('addItemFav', true);
      }
        catch (e){
          console.error(e);
        }
      } else if(this.checkAvailable){
        const item = {
        id: el.id
      }
      this.$store.commit('favorite/remove', item)
        this.$emit('addItemFav', true);
      }
    },
    async toCart(el){
      try{
        let respCom = await this.$axios.get(this.$config.baseURL + '/api/site/catalog/' + el.id);
        const dataCom = respCom.data.data.filters;
        // console.log(dataCom);
        let depth = '';
        dataCom.forEach(item => {
          if(item.name === 'Глубина'){
            depth = item.value
          }
        });
        // console.log('Высота ' + height)
        // console.log('Ширина ' + width)
        // console.log('Глубина ' + depth)
        // console.log('Длина ' + lengthItem)
        const item = {
          code: el.id,
          name: el.name,
          img: el.images[0],
          price: el.price,
          old_price: el.price_old,
          brand: el.brand,
          count: 1,
          type: el.type,
          width: el.width,
          height: el.hight,
          depth,
          lengthItem: el.length,
        }
        this.$store.commit('cart/add', item);
        this.$emit('addItemCart', true);
      } catch (e){
        console.error(e);
      }
      
    // console.log('Высота ' + height)
    // console.log('Ширина ' + width)
    // console.log('Глубина ' + depth)
    // console.log('Длина ' + lengthItem)
    // let item = {
    //   code: this.el.id,
    //   name: this.el.name,
    //   img: this.el.images[0],
    //   price: this.el.price,
    //   old_price: this.el.price_old,
    //   brand: this.el.brand,
    //   count: 1,
    //   type: this.el.type,
    //   width: this.el.width,
    //   height: this.el.height,
    //   depth: this.el.depth,
    //   lengthItem: '',
    // };
    // this.$store.commit('cart/add', item);
  }
  }
};
</script>

<style lang="scss">
#s-card-good-small{
  width: 100% !important;
  border-top: unset !important;
  border-left: unset !important;
  border-right: unset !important;
  &:hover{
    box-shadow: unset !important;
  }
}
.wrapper{
  flex-grow: 1;
  margin-top: auto !important;
}
.s-item-list-icons {
  p{
    font-size: 12px !important;
    color: rgb(129, 129, 129);
    letter-spacing: normal !important;
    text-transform: none !important;
  }
  button{
    border: unset !important;
    background-color: unset !important;
  }
}
.s-item-list-id{
  font-size: 14px !important;
}
.s-item-list-size{
  span{
    font-size: 12px !important;
  }
}
.s-item-list-icons{
  img{
    width: 1.2em !important;
  }
}

.s-item-list-price{
  >.s-item-list-price-main{
    font-size: 25px !important;
  }

}
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
      font-size: 8px !important;
      min-width: unset !important;
    }
  }
}

@media screen and (max-width: 540px) {
  .s-item-list-img{
    width: 120px !important;
    height: 120px !important;
  }
  .s-item-list-icons{
    width: 120px !important;
  }
  .s-item-list-price{
    .s-item-list-price-second{
      font-size: 17px !important;
    }
    >.s-btn-cart{
      span{
        font-size: 12px !important;
      }
    }
  }
}
@media screen and (max-width: 466px) {
  .s-item-list-img{
    width: 100px !important;
    height: 140px !important;
    max-height: 100% !important;
  }
  .s-item-list-icons{
    width: 100px !important;
  }
  .s-item-list-price{
    .s-item-list-price-second{
      font-size: 15px !important;
    }
    >.s-btn-cart{
      span{
        font-size: 10px !important;
      }
    }
  }
}
@media screen and (max-width: 456px) {
  .s-item-list-price{
    >.s-item-list-price-main{
      font-size: 19px !important;
    }
    .s-btn-cart{
      font-size: 10px !important;
      max-width: 110px !important;
    }
  }
}
@media screen and (max-width: 425px) {
  .s-item-list-available{
    span{
      font-size: 12px !important;  
    }
  }
  .s-item-list-id{
    font-size: 12px !important;
  }
  .s-item-list-size{
    span{
      font-size: 12px !important;
    }
  }
  
  .s-item-list-price{
    >.s-item-list-price-main{
      font-size: 23px !important;
    }
    >.s-btn-cart{
      span{
        font-size: 9px !important;
      }
    }
  }
}
@media screen and (max-width: 404px) {
  .s-item-list-price{
    .s-item-list-price-second{
      font-size: 12px !important;
    }
  }
}
</style>

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
  .s-item-list-brand{
    
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