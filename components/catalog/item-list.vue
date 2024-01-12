<template>
  <v-card class="s-card-good pa-4">
    <div>
      <nuxt-link v-if="el.isparent !== 0" :to="'/catalog/view/' + el.id">
        <div class="mb-2" style="position: relative;">
          <v-img v-if="el.images && el.images[0]" :contain="true" style="width: 400px; height: 250px" :src="$config.baseImageURL+el.images[0]+'?height=250'" />
        </div>
        <div class="d-flex flex-column flex-md-row justify-space-between mb-2">
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
            <span style="color: #949494">Бренд: </span><span>{{ el.brand }}</span>
          </div>
        </div>
        <div class="my-2 s-item-list-price" style=" font-weight: bold;">
          <span v-if="el.price" style="font-size: 20px">
            <number :value="el.price" /> ₽
          </span>
          <span v-else style="font-size: 20px">
            Цена не указана
          </span>
          <span class="ml-0" v-if="el.price_old"
            style="font-size: 13px; text-decoration: line-through; color: #949494">
            <number :value="el.price_old" /> ₽
          </span>
        </div>
      </nuxt-link>
      <nuxt-link v-else :to="'/catalog/' + el.id">
        <div style="position: relative;" class="mb-2">
          <v-img v-if="el.images && el.images[0]" :contain="true" style="width: 400px; height: 250px" :src="$config.baseImageURL+el.images[0]+'?height=250'" />
        </div>
        <div v-if="el.isparent !== 0" class="d-flex justify-space-between mb-2">
          <div class="s-item-list-available" style="margin: 3px 0; font-size: 13px">
            <catalog-available :value="el.store" />
          </div>
          <div class="s-item-list-id" style="margin: 3px 0; font-size: 13px; color: #949494">
            Код товара: {{ el.id }}
          </div>
        </div>
        <div class="mb-4 s-item-list-name" style="margin: 3px 0; font-size: 16px; font-weight: bold" :class="{ 'hidden-text': hiddentext }">{{ el.name }}</div>
        <div class="my-1 s-item-list-size" style="font-size: 11px">
          <div v-if="el.width || el.depth || el.height">
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
            <span v-if="el.brand" style="color: #949494">Бренд: </span><span>{{ el.brand }}</span>
          </div>
        </div>
        <div class="my-2 s-item-list-price" v-if="el.isparent !== 0" style=" font-weight: bold;">
          <span v-if="el.price" style="font-size: 20px">
            <number :value="el.price" /> ₽
          </span>
          <span v-else style="font-size: 20px">
            Цена не указана
          </span>
          <span class="ml-0" v-if="el.price_old"
            style="font-size: 13px; text-decoration: line-through; color: #949494">
            <number :value="el.price_old" /> ₽
          </span>
        </div>
      </nuxt-link>
    </div>
    <div v-if="el.isparent !== 0" class="d-flex justify-space-between s-item-list-btn">
      <v-btn @click="toCart(el)" dark class="s-btn-cart s-btn-text">В корзину</v-btn>
      <div class="d-flex flex-row ms-5 s-item-list-icons">
        <v-btn @click="toFavorite(el)" icon>
          <img v-show="checkAvailable" src="/icons/like-black.svg" alt="favorite-black">
          <img v-show="!checkAvailable" src="/icons/like.svg" alt="favorite">
        </v-btn>
        <v-btn @click="toCompare(el)" icon><img src="/icon-similar.png" alt="" /></v-btn>
      </div>
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
    },
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