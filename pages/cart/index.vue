<template>
    <v-container class="mb-10 s-cart-parent">
        <v-divider class="mb-8" />

        <v-snackbar v-model="snackbarCart">{{ dataResultCart }} <template v-slot:action="{ attrs }">
            <v-btn color="pink" text v-bind="attrs" @click="snackbarCart = false">
              Закрыть
            </v-btn>
          </template>
          </v-snackbar>

        <common-beadcrumbs class="mb-4" :value="breadcrumbsData" />
        <div class="d-flex flex-row justify-space-between align-center s-static-main">
            <h1>{{ title }}</h1>
            <v-btn v-if="cart.length > 0" @click="removeAll" outlined class="mb-5 pt-2 pb-2 clearBtn d-none d-sm-block">Очистить корзину <img src="/icons/del_card.svg" class="del_card ms-2" /></v-btn>
        </div>
        <v-divider class="mb-8" />
        <div v-if="cart.length == 0" style="padding: 120px 0;" class="text-center s-fav-empty">
            <div style="font-weight: bold; font-size: 46px;">Ваша корзина пуста</div>
            <div style="font-size: 20px; margin: 5px 0 30px 0;">Перейдите в каталог</div>
            <div>
              <v-btn dark to="/catalog/156">В каталог</v-btn>
            </div>
          </div>
        <div v-else>
            <v-row class="d-none">
                <v-col cols="5">
                    <div class="grey--text">Товар</div>
                </v-col>
                <v-col cols="2">
                    <div class="grey--text">Стоимость</div>
                </v-col>
                <v-col cols="2">
                    <div class="grey--text">Количество</div>
                </v-col>
                <v-col cols="2">
                    <div class="grey--text">Цена</div>
                </v-col>
                <v-col cols="1">
                    <div class="grey--text"></div>
                </v-col>
            </v-row>

            <div v-for="(el, i) in cart" :key="i" class="align-center">
                <v-row>
                    <v-col class="col-12 d-none d-sm-block col-sm-9 pl-0">
                        <div class="d-flex">
                            <div class="mr-4 pa-2" style="border: 0.5px solid black">
                                <v-img style="width: 100px; height: 100px;" contain :src="$config.baseImageURL+el.img+'?width=250&height=250'" />
                            </div>
                            <div>
                                <div style="font-size: 13px" class="mb-2 grey--text">Код товара: {{ el.code }}</div>
                                <div @click="toItem(el)" style="font-size: 16px" class="mb-2 toItemblock">{{ el.name }}</div>
                                <!-- <div v-if="el.depth !== '' " style="font-size: 13px"><span class="grey--text mr-2">Габариты
                                        (Г.Ш.В):</span><span>{{`${el.depth} x ${el.width} x ${el.height}` }}</span></div>
                                <div v-else style="font-size: 13px"><span class="grey--text mr-2">Габариты
                                        (Д.Ш.В):</span><span>{{`${el.lengthItem} x ${el.width} x ${el.height}` }}</span></div> -->
                                <div style="font-size: 13px">
                                    <span class="grey--text mr-2">Бренд:</span>
                                    <span>{{el.brand}}</span>
                                </div>

                                <div style="font-size: 13px" v-if="el.height && el.width && el.length"><span class="grey--text mr-2">Габариты (Д.Ш.В): </span>
                                    <span>{{`${el.length} x ${el.width} x ${el.height}` }}</span>
                                  </div>
                                  <div style="font-size: 13px" v-else-if="!el.length && el.width && el.height"><span class="grey--text mr-2">Габариты (Ш.В): </span>
                                    <span>{{`${el.width} ${' x ' + el.height}` }}</span>
                                  </div>
                                  <div style="font-size: 13px" v-else-if="!el.width && el.length && el.height"><span class="grey--text mr-2">Габариты (Д.В): </span>
                                    <span>{{`${el.length}${' x ' + el.height}` }}</span>
                                  </div>
                                  <div style="font-size: 13px" v-else-if="!el.height && el.length && el.width"><span class="grey--text mr-2">Габариты (Д.Ш): </span>
                                    <span>{{`${el.length} x ${el.width}` }}</span>
                                  </div>
                                  <div style="font-size: 13px" v-else><span class="grey--text mr-2">Габариты (Д.Ш.В): </span>
                                    <span>Не указаны</span>
                                  </div>


                            </div>
                        </div>
                    </v-col>
                     <!-- <600px -->
                    <v-col class="col-12 col-sm-3 d-none d-sm-none justify-end pr-0">
                        <!-- <v-col class="col-9 col-sm-auto d-flex justify-space-between pr-0">
                            <v-col class="col-4 col-sm-12 d-flex d-md-block justify-space-between align-center">
                                <div style="font-size: 20px">
                                    <b><number :value="el.price" /> ₽</b>
                                </div>
                                
                            </v-col>
                            <v-col class="col-4 col-sm-12 pa-0">
                                <div>
                                    <div class="mb-2 d-flex justify-space-between" v-if="el.type == 2">
                                        <v-btn class="s-btn-text" dark>м<sup>2</sup></v-btn>
                                        <v-btn class="s-btn-text">упак</v-btn>
                                    </div>
                                    <v-text-field hide-details class="s-input-text-center" outlined dense v-model="el.count">
                                        <v-btn @click="countPlus(el)" style="margin-top: -6px;" slot="append" icon><i
                                                class="fa-solid fa-plus"></i></v-btn>
                                        <v-btn @click="countMinus(el)" style="margin-top: -6px;"
                                            slot="prepend-inner" icon><i class="fa-solid fa-minus"></i></v-btn>
                                    </v-text-field>
                                </div>
                            </v-col>
                            <v-col class="col-3">
                                <div>
                                    <v-btn icon><img @click="deleteItem(el)" src="/icons/trash.png" alt="removeitem-icon"/></v-btn>
                                </div>
                            </v-col>
                        </v-col> -->
                        <div class="wrapper"></div>
                        <div class="my-2 my-sm-0 d-flex flex-row justify-space-between align-center s-item-list-price" style=" font-weight: bold;">
                        <div class="d-flex flex-column align-end">
                            <div style="font-size: 20px">
                                <b><number :value="el.price" /> ₽</b>
                            </div>
                        </div>
                        <v-col class="col-5 col-sm-12 pa-0">
                            <div>
                                <div class="mb-2 d-flex justify-space-between" v-if="el.type == 2">
                                    <v-btn class="s-btn-text" dark>м<sup>2</sup></v-btn>
                                    <v-btn class="s-btn-text">упак</v-btn>
                                </div>
                                <v-text-field hide-details class="s-input-text-center" outlined dense v-model="el.count">
                                    <v-btn @click="countPlus(el)" style="margin-top: -6px;" slot="append" icon><i
                                            class="fa-solid fa-plus"></i></v-btn>
                                    <v-btn @click="countMinus(el)" style="margin-top: -6px;"
                                        slot="prepend-inner" icon><i class="fa-solid fa-minus"></i></v-btn>
                                </v-text-field>
                            </div>
                        </v-col>
                        </div>
                        <!-- Блок под вопросом -->
                        <v-col class="d-none d-md-block col-2">
                            <div>
                                <div style="font-size: 22px">
                                    <b><number :value="el.count * el.price" /> ₽</b>
                                </div>
                                <div v-if="el.old_price" style="font-size: 16px; text-decoration: line-through;"
                                    class="grey--text">
                                    <b><number :value="el.count * el.old_price" /> ₽</b>
                                </div>
                            </div>
                        </v-col>
                    </v-col>
                    <!-- >600px -->
                    <v-col class="col-3 d-none d-sm-block justify-end pr-0">
                            <v-col class="col-sm-12 d-flex d-md-block justify-space-between align-center pr-0">
                                <div class="d-flex flex-column align-start">
                                    <b style="font-size: 18px">
                                        <number :value="el.price" />
                                         ₽
                                    </b>
                                    <div v-if="el.count>1">
                                        <div style="font-size: 12px; color: #949494">
                                            <b> {{ el.count }} x <number :value="el.count * el.price" /> ₽</b>
                                        </div>
                                        <div v-if="el.old_price" style="font-size: 12px; text-decoration: line-through;"
                                            class="grey--text">
                                            <b><number :value="el.count * el.old_price" /> ₽</b>
                                        </div>
                                    </div>
                                </div>
                                <div class="d-none d-sm-block d-md-none">
                                    <v-btn icon><img @click="deleteItem(el)" src="/icons/trash.png" alt="removeitem-icon"/></v-btn>
                                </div>
                            </v-col>
                            <v-col class="col-sm-12 pr-0">
                                <div>
                                    <div class="mb-2 d-flex justify-space-between" v-if="el.type == 2">
                                        <v-btn class="s-btn-text" dark>м<sup>2</sup></v-btn>
                                        <v-btn class="s-btn-text">упак</v-btn>
                                    </div>
                                    <v-text-field hide-details class="s-input-text-center" outlined dense v-model="el.count">
                                        <v-btn @click="countPlus(el)" style="margin-top: -6px;" slot="append" icon><i
                                                class="fa-solid fa-plus"></i></v-btn>
                                        <v-btn @click="countMinus(el)" style="margin-top: -6px;"
                                            slot="prepend-inner" icon><i class="fa-solid fa-minus"></i></v-btn>
                                    </v-text-field>
                                </div>
                            </v-col>
                        <v-col class="d-none d-md-block col-2">
                            <div>
                                <div style="font-size: 22px">
                                    <b><number :value="el.count * el.price" /> ₽</b>
                                </div>
                                <div v-if="el.old_price" style="font-size: 16px; text-decoration: line-through;"
                                    class="grey--text">
                                    <b><number :value="el.count * el.old_price" /> ₽</b>
                                </div>
                            </div>
                        </v-col>
                        <v-col cols="12" class="d-none d-md-block">
                            <div>
                                <v-btn icon><img @click="deleteItem(el)" src="/icons/trash.png" alt="removeitem-icon"/></v-btn>
                            </div>
                        </v-col>
                    </v-col>
                    
                    <v-card id="s-card-good-small" class="s-card-good d-sm-none py-5">
                        <div>
                          <v-col class="pa-0 col-12 d-flex flex-row justify-space-between">
                            <v-col class="mb-2 pa-0 pr-4 d-flex flex-column align-start col-4" style="position: relative;">
                              <div class="mr-4 pa-1" style="border: 0.5px solid black">
                                <v-img class="s-item-list-img" contain style="width: 100px; height: 100px;" :src="$config.baseImageURL+el.img+'?width=250&height=250'" />
                            </div>
                              <!-- <div class="d-flex flex-column align-start mt-1 s-item-list-icons">
                                <v-btn @click="toFavorite(el)" class="pa-0">
                                  <img v-show="checkAvailable" src="/icons/like-black.svg" alt="favorite-black">
                                  <img v-show="!checkAvailable" src="/icons/like.svg" alt="favorite">
                                  <p class="ma-0 pa-0 ms-2">В избранное</p>
                                </v-btn>
                                <v-btn @click="toCompare(el)" class="pa-0">
                                  <img src="/icon-similar.png" alt="" />
                                  <p class="ma-0 pa-0 ms-2">Сравнить</p>
                                </v-btn>
                              </div> -->
                            </v-col>
                            <v-col class="pa-0 pl-3">
                                <div class="d-flex flex-column" style="height: 100%;">
                                    
                                <div class="d-flex flex-row justify-space-between mb-2">
                                  <!-- <div class="s-item-list-available" style="margin: 3px 0; font-size: 13px">
                                    <catalog-available :value="el.store" />
                                  </div> -->
                                  <div class="s-item-list-id" style="margin: 3px 0; font-size: 13px; color: #949494">
                                    Код товара: {{ el.code }}
                                  </div>
                                </div>
                                <nuxt-link :to="'/catalog/view/' + el.code">
                                    <div class="mb-4 s-item-list-name" style="margin: 3px 0; font-size: 16px; font-weight: bold">{{ el.name }}</div>
                                </nuxt-link>
                                <div class="my-1 s-item-list-size" style="font-size: 11px">
                                  <div>
                                    <!-- <span style="color: #949494">Габариты (Д.Ш.В): </span><span>{{ el.width }}</span> -->
                                    <!-- <div v-if="itemList.depth && itemList.height"><span style="color: #949494">Габариты (Г.Ш.В): </span>
                                      <span>{{`${itemList.depth + ' x '} ${itemList.width} ${' x ' + itemList.height}` }}</span>
                                    </div>
                                    <div v-else-if="!itemList.height"><span style="color: #949494">Габариты (Г.Ш): </span>
                                      <span>{{`${itemList.depth + ' x '} ${itemList.width}`}}</span>
                                    </div> -->
                                    <div style="font-size: 13px" v-if="el.height && el.width && el.length"><span class="grey--text mr-2">Габариты (Д.Ш.В): </span>
                                        <span>{{`${el.length} x ${el.width} x ${el.height}` }}</span>
                                      </div>
                                      <div style="font-size: 13px" v-else-if="!el.length && el.width && el.height"><span class="grey--text mr-2">Габариты (Ш.В): </span>
                                        <span>{{`${el.width} ${' x ' + el.height}` }}</span>
                                      </div>
                                      <div style="font-size: 13px" v-else-if="!el.width && el.length && el.height"><span class="grey--text mr-2">Габариты (Д.В): </span>
                                        <span>{{`${el.length}${' x ' + el.height}` }}</span>
                                      </div>
                                      <div style="font-size: 13px" v-else-if="!el.height && el.length && el.width"><span class="grey--text mr-2">Габариты (Д.Ш): </span>
                                        <span>{{`${el.length} x ${el.width}` }}</span>
                                      </div>
                                      <div style="font-size: 13px" v-else><span class="grey--text mr-2">Габариты (Д.Ш.В): </span>
                                        <span>Не указаны</span>
                                      </div>
                                      
                                      
                                      
                                  </div>
                                  <div class="s-item-list-brand">
                                    <span style="color: #949494">Бренд: </span><span>{{ el.brand }}</span>
                                  </div>
                                </div>
                                <!-- <div class="wrapper"></div> -->
                                <div class="my-2 my-sm-0 d-flex flex-row align-end justify-space-between s-item-list-price" style=" font-weight: bold;">
                                  <div class="d-flex flex-column align-start">
                                    <span v-if="el.price" class="s-item-list-price-main" style="font-size: 18px">
                                      <number :value="el.price" /> ₽
                                    </span>
                                    <span v-else class="s-item-list-price-second" style="font-size: 18px">
                                      Цена не указана
                                    </span>
                                    <span class="mx-2" v-if="el.price_old" style="font-size: 13px; text-decoration: line-through; color: #949494">
                                        <number :value="el.price_old" /> ₽
                                      </span>
                                        <div v-if="el.count>1 && el.price>0">
                                            <div style="font-size: 12px; color: #949494">
                                                <b> {{ el.count }} x <number :value="el.count * el.price" /> ₽</b>
                                            </div>
                                            <div v-if="el.old_price" style="font-size: 12px; text-decoration: line-through;"
                                                class="grey--text">
                                                <b><number :value="el.count * el.old_price" /> ₽</b>
                                            </div>
                                        </div>
                                  </div>
                                  <v-col class="col-5 pa-0">
                                    <div>
                                        <div class="mb-2 d-flex justify-space-between" v-if="el.type == 2">
                                            <v-btn class="s-btn-text" dark>м<sup>2</sup></v-btn>
                                            <v-btn class="s-btn-text">упак</v-btn>
                                        </div>
                                        <v-text-field hide-details class="s-input-text-center" outlined dense v-model="el.count">
                                            <v-btn class="s-btn-cart-field" @click="countPlus(el)" slot="append" icon>
                                                <i class="fa-solid fa-plus"></i>
                                            </v-btn>
                                            <v-btn class="s-btn-cart-field" @click="countMinus(el)" slot="prepend-inner" icon>
                                                <i class="fa-solid fa-minus"></i>
                                            </v-btn>
                                        </v-text-field>
                                    </div>
                                </v-col>
                                  <div>
                                    <v-btn style="width: 20px; height: 20px" icon>
                                        <img @click="deleteItem(el)" src="/icons/trash.png" alt="removeitem-icon"/>
                                    </v-btn>
                                </div>
                                </div>
                              </div>
                              </v-col>
                          </v-col>
                        </div>
                      </v-card>
                </v-row>
                <!-- <v-divider class="my-8" /> -->
            </div>
        </div>
        <div class="my-8 s-cart-footer">
            <v-row v-if="cart.length !== 0">
                <v-col cols="12" class="px-0">
                        <v-text-field class="s-input-cart-footer" outlined hide-details placeholder="Введите промо-код" style="max-width: 254px">
                            <v-btn style="margin-top: -8px;" slot="append" fab dark small><i
                                    class="fa-solid fa-arrow-right"></i>
                            </v-btn>
                        </v-text-field>
                </v-col>
                <v-col cols="12" class="d-none d-sm-flex justify-space-between px-0">
                        <div class="d-flex align-center justify-end">
                            <div class="mr-3" style="font-size: 15px;">
                                <b>Общая стоимость:</b>
                            </div>
                            <div>
                                <div style="font-size: 25px">
                                    <b><number :value="totalPrice" /> ₽</b>
                                </div>
                                <div v-if="totalDiscount && totalDiscount !== 0" style="font-size: 14px" class="red--text">Экономия:
                                    <number :value="totalDiscount" /> ₽
                                </div>
                            </div>
                        </div>
                        <div>
                            <div class="mb-4">
                                <nuxt-link to="/cart/order"><v-btn class="s-btn-text" style="width: 100%" dark>Оформить заказ</v-btn></nuxt-link>
                            </div>
                            <div>
                                <span class="green--text">
                                    <span class="green mr-2"
                                    style="color: #fff;
                                    border-radius: 50%;
                                    width: 28px;
                                    display: inline-block;
                                    height: 28px;
                                    text-align: center;
                                    padding-top: 2px;">
                                        <img src="/icons/timer.png" />
                                    </span>
                                В рассрочку 
                                </span>
                                <span><b style="text-decoration: underline;">от 2 460 ₽/мес.</b></span>
                            </div>
                        </div>
                </v-col>
                <v-col cols="12" class="d-sm-none px-0">
                    <v-col class="col-12 d-flex align-center justify-start px-0">
                        <div class="mr-3" style="font-size: 15px;">
                            <b>Общая стоимость:</b>
                        </div>
                        <div>
                            <div style="font-size: 25px">
                                <b><number :value="totalPrice" /> ₽</b>
                            </div>
                            <div v-if="totalDiscount && totalDiscount !== 0" style="font-size: 14px" class="red--text">Экономия:
                                <number :value="totalDiscount" /> ₽
                            </div>
                        </div>
                    </v-col>
                    <v-col class="col-12 d-flex flex-column align-center px-0">
                        <div class="mb-4 w-100">
                            <nuxt-link to="/cart/order"><v-btn class="s-btn-text" style="width: 100%" dark>Оформить заказ</v-btn></nuxt-link>
                        </div>
                        <div>
                            <span class="green--text">
                                <span class="green mr-2"
                                style="color: #fff;
                                border-radius: 50%;
                                width: 28px;
                                display: inline-block;
                                height: 28px;
                                text-align: center;
                                padding-top: 2px;">
                                    <img src="/icons/timer.png" />
                                </span>
                            В рассрочку 
                            </span>
                            <span><b style="text-decoration: underline;">от 2 460 ₽/мес.</b></span>
                        </div>
                    </v-col>
                 </v-col>
                <v-col cols="6" class="d-none justify-end align-end">
                    <v-col cols="10">
                        <div class="mb-4">
                            <nuxt-link to="/cart/order"><v-btn class="s-btn-text" style="width: 100%" dark>Оформить заказ</v-btn></nuxt-link>
                        </div>
                        <div>
                            <span class="green--text">
                                <span class="green mr-2"
                                style="color: #fff;
                                border-radius: 50%;
                                width: 28px;
                                display: inline-block;
                                height: 28px;
                                text-align: center;
                                padding-top: 2px;">
                                <img src="/icons/timer.png" />
                                </span>
                            В рассрочку 
                            </span>
                            <span><b style="text-decoration: underline;">от 2 460 ₽/мес.</b></span>
                        </div>
                    </v-col>
                </v-col>
            </v-row>
        </div>
    </v-container>
</template>

<script>
import number from '../../components/number.vue';
import { mapGetters } from 'vuex'
export default {
    components: { number },
    data() {
        return {
            mass: [],
            snackbarCart: false
        }
    },
    async asyncData(params) {
        const title = "Корзина"
        const breadcrumbsData = [
            {
                url: "",
                title: title,
            }
        ];
        // const data = [
        //     {
        //         id: 100,
        //         name: "Название товара",
        //         image: ["/img/cart/1.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         count: 1,
        //     },
        //     {
        //         id: 100,
        //         type: 2,
        //         name: "Название товара",
        //         image: ["/img/cart/2.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         count: 1.2,
        //     },
        // ]
        return { title, breadcrumbsData }
    },
    computed: {
        ...mapGetters ({
      totalPrice: 'cart/totalPrice',
      totalDiscount: 'cart/totalDiscount',
      cart: 'cart/cart',
      dataResultCart: 'cart/dataResult',
    }),
    },
    methods: {
        deleteItem(el){
            this.$store.commit('cart/remove', el)
            this.snackbarCart = true
        },
        toItem(el){
            this.$router.push({path: '/catalog/view/' + el.code})
        },
        removeAll(){
            this.$store.commit('cart/removeAll');
            this.snackbarCart = true
        },
        countPlus(el){
            this.$store.commit('cart/countPlus', el.code)
        },
        countMinus(el){
            this.$store.commit('cart/countMinus', el.code)
        }
    }
}

</script>


<style lang="scss">
@media screen and (max-width: 600px) {
    .s-cart-footer{
        .s-input-cart-footer{
            max-width: 100% !important;
        }
    }
}
.v-btn--icon{
    color: black !important;
}
.s-input-text-center{
    i{
        width: 1.2em;
    }
    input {
        font-size: 14px !important;
        text-align: center;
    }
    .v-input__slot{

    }
    .s-btn-cart-field{
        width: 20px;
        height: 20px;
    }
} 
.toItemblock:hover{
        cursor: pointer;
}
.clearBtn{
    background-color: rgb(243, 243, 243);
    border: unset;
    .v-btn__content{
        text-transform: none;
        letter-spacing: 0px;
    }
}
.del_card{
    width: 16px;
    height: 16px;
}
</style>