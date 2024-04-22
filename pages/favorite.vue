<template>
    <v-container class="mb-10">

        <!-- Избранное -->
<v-snackbar v-model="snackbarFav">{{ dataResultFav }} <template v-slot:action="{ attrs }">
    <v-btn color="pink" text v-bind="attrs" @click="snackbarFav = false">
      Закрыть
    </v-btn>
  </template>
  </v-snackbar>
<!-- Сравнение -->
  <v-snackbar v-model="snackbarCom">{{ dataResultCom }} <template v-slot:action="{ attrs }">
    <v-btn color="pink" text v-bind="attrs" @click="snackbarCom = false">
      Закрыть
    </v-btn>
  </template>
  </v-snackbar>
  <!-- Корзина -->
          <v-snackbar v-model="snackbarCart">{{ dataResultCart }} <template v-slot:action="{ attrs }">
    <v-btn color="pink" text v-bind="attrs" @click="snackbarCart = false">
      Закрыть
    </v-btn>
  </template>
  </v-snackbar>


        <v-divider class="mb-8" />
        <common-beadcrumbs class="mb-4" :value="breadcrumbsData" />
        <div class="d-flex justify-space-between s-static-main">
            <h1>{{ title }}</h1>
            <div class="s-clear-btn">
                <v-btn v-show="dataFav.length !== 0" @click="removeAll" outlined class="mb-5 pt-2 pb-2 clearBtn">Очистить список <img src="/icons/del_card.svg" class="del_card ms-2" /></v-btn>
            </div>
        </div>
        <v-divider class="mb-10 s-fav-divider" :class="{ 'd-block': dataFav.length == 0 }" />
        <div v-show="dataFav.length == 0" style="padding: 120px 0;" class="text-center s-fav-empty">
            <div style="font-weight: bold; font-size: 46px;">В избранном пусто</div>
            <div style="font-size: 20px; margin: 5px 0 30px 0;">Перейдите в каталог</div>
            <div>
              <v-btn dark to="/allcategories">В каталог</v-btn>
            </div>
          </div>
        <div v-show="dataFav.length !== 0">
            <v-row class="s-row s-row-catalog px-0">
                <v-row>
                    <v-col class="col-4 px-2" v-for="(el, i) in dataFav" :key="i">
                        <favorite-item-list :el="el" @removeItemFav="removeItem" @addItemCart="addItemCart" @addItemCom="addItemCom" />
                    </v-col>
                </v-row>
                <v-row class="d-none s-row-catalog-small">
                    <v-col class="pa-0" cols="12" v-for="(el, i) in dataFav" :key="i">
                      <favorite-item-list-small :el="el" @removeItemFav="removeItem" @addItemCart="addItemCart" @addItemCom="addItemCom" />
                    </v-col>
                  </v-row>
            </v-row>
        </div>
    </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
    data() {
        return {
            snackbarFav: false,
            snackbarCom: false,
            snackbarCart: false
        }
    },
    computed: {
        ...mapGetters({
            dataFav: 'favorite/favItems',
            dataResultFav: 'favorite/dataResult',
            dataResultCom: 'compare/dataResult',
            dataResultCart: 'cart/dataResult',
        }),
    },
    methods: {
        removeItem(val){
            this.snackbarCart = false;
            this.snackbarCom = false;
            this.snackbarFav = val;
        },
        removeAll(){
            this.$store.commit('favorite/removeAll');
            this.snackbarCart = false;
            this.snackbarCom = false;
            this.snackbarFav = true;
        },
        addItemCart(el){
            this.snackbarFav = false;
            this.snackbarCom = false;
            this.snackbarCart = el;
        },
        addItemCom(el){
            this.snackbarFav = false;
            this.snackbarCart = false;
            this.snackbarCom = el;
        }
    },
    async asyncData(params) {
        const title = "Избранные товары"
        const breadcrumbsData = [
            {
                url: "",
                title: title,
            }
        ];

        return { title, breadcrumbsData }
    }
}
</script>

<style lang="scss">
@media screen and (max-width: 600px) {
    .s-fav-empty{
        >div:first-child{
            font-size: 40px !important;
        }
        >div:nth-child(2){
            font-size: 18px !important;
        }
    }
    
}
@media screen and (max-width: 425px) {
    .s-fav-empty{
        >div:first-child{
            font-size: 30px !important;
        }
        >div:nth-child(2){
            font-size: 15px !important;
        }
    }
    
}
</style>