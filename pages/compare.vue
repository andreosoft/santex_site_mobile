<template>
    <v-container class="mb-10">
        <v-divider class="mb-8" />

        <!-- Сравнение -->
        <v-snackbar v-model="snackbarCom">
            {{ dataResultCom }} 
            <template v-slot:action="{ attrs }">
            <v-btn color="pink" text v-bind="attrs" @click="snackbarCom = false">
              Закрыть
            </v-btn>
          </template>
          </v-snackbar>
<!-- Избранное -->
<v-snackbar v-model="snackbarFav">
    {{ dataResultFav }} 
    <template v-slot:action="{ attrs }">
    <v-btn color="pink" text v-bind="attrs" @click="snackbarFav = false">
      Закрыть
    </v-btn>
  </template>
  </v-snackbar>
  <!-- Корзина -->
          <v-snackbar v-model="snackbarCart">
            {{ dataResultCart }}
            <template v-slot:action="{ attrs }">
    <v-btn color="pink" text v-bind="attrs" @click="snackbarCart = false">
      Закрыть
    </v-btn>
  </template>
  </v-snackbar>
  
        <common-beadcrumbs class="mb-4" :value="breadcrumbsData" />
        <div class="d-flex justify-space-between s-static-main">
            <h1>{{ title }}</h1>
            <div class="d-none d-sm-block">
                <v-btn v-show="dataCom.length !== 0" @click="removeAll" outlined class="mb-5 pt-2 pb-2 clearBtn">Очистить список <img src="/icons/del_card.svg" class="del_card ms-2" /></v-btn>
            </div>
        </div>
        <v-divider v-show="dataCom.length == 0" class="mb-10 d-md-block" />
        <div v-show="dataCom.length == 0" style="padding: 120px 0;" class="text-center s-fav-empty">
            <div style="font-weight: bold; font-size: 46px;">В сравнении товаров пусто</div>
            <div style="font-size: 20px; margin: 5px 0 30px 0;">Перейдите в каталог</div>
            <div>
              <v-btn dark to="/catalog/allcategories">В каталог</v-btn>
            </div>
        </div>
        <div v-show="dataCom.length !== 0">
            <div class="d-flex flex-column flex-md-row">
                <div class="d-none d-md-block">
                    <div class="mb-10" style="height: 547.5px; border: 1px solid #DBDBDB; padding: 60px 20px 30px 20px;">
                        <div class="mb-8" style="font-size: 20px; font-weight: bold;">
                            Добавлено: {{ countCom }} шт.
                        </div>
                        <v-divider class="mb-8" />
                        <div>
                            <div><b>Список товаров</b></div>
                            <div>
                                <v-select :items="paramsCom" v-model="valueList"></v-select>
                            </div>
                        </div>
                        <div style="margin-bottom: 120px;">
                            <div :class="noDifference">
                                <p @click="activeDifference"><i class="fa-regular" :class="hasDistinction"></i> Только отличия</p>
                            </div>
                            <div class="triggerInput">
                                <p @click="activeAllParams"><i class="fa-regular" :class="{'fa-circle-check': allParamInput, 'fa-circle': !allParamInput}"></i> Все параметры</p>
                            </div>
                        </div>
                        <div>
                            <img src="/logo2.png" />
                        </div>
                    </div>
                    <div>
                        <div v-if="differenceInput" class="s-comapre-table">
                            <div v-for="(parametr, i) in differenceItems" class="s-comapre-table-row grey--text">
                                <div>
                                    {{ parametr }}
                                </div>
                            </div>
                        </div>
                        <div v-else class="s-comapre-table">
                            <div v-for="(parametr, i) in dataParameters" class="s-comapre-table-row grey--text">
                                <div>
                                    {{ parametr }}
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <v-col style="overflow: auto;" cols="12" class="w-100 flex-column px-0 d-flex mb-8 s-catalog-links-compare">
                    <div class="d-flex w-100 s-catalog-links-common">
                        <v-col
                        cols="auto"
                        v-for="(el, i) in paramsCom" 
                        :key="i" 
                        class="s-catalog-links-el pb-0 mr-8 d-flex flex-row"
                        @click="valueList = el"
                        :class="{active: el == valueList}"
                        >
                        <p>{{ el }}</p>
                        <span v-if="el == 'Все товары'" class="grey--text ml-3">{{ dataCom.length }}</span>
                        <span v-else class="grey--text ml-3">{{ otherLength(el) }}</span>
                        </v-col>
                    </div>
                <!-- <div class="s-catalog-hr w-100 ma-0 pa-0"></div> -->
                </v-col>
                    <div class="d-md-none d-flex flex-row">
                        <div :class="noDifference" class="mr-4">
                            <p @click="activeDifference">
                                <i class="fa-regular mr-2" :class="hasDistinction"></i>
                                Только отличия
                            </p>
                        </div>
                        <div class="triggerInput">
                            <p @click="activeAllParams">
                                <i 
                                class="fa-regular mr-2" 
                                :class="{'fa-circle-check': (!differenceInput) || (dataDifFilters.length == 0), 'fa-circle': (differenceInput) && (dataDifFilters.length > 0)}">
                                </i> 
                                Все параметры
                            </p>
                        </div>
                    </div>
                <div style="overflow: auto">
                    <div>
                        <div class="d-flex s-compare-items">
                            <div style="min-width: 220px !important; max-width: 220px !important" v-for="(item, ind) in visibleItems" :key="ind">
                                <div class="mb-10 w-100">
                                <catalog-item-list-compare :el="item" @removeItemCom="removeItem" @addItemFav="addItemFav" @addItemCart="addItemCart" />
                                </div>
                                <div v-if="differenceInput" class="s-comapre-table" :class="{'mb40': ind > 0}">
                                    <div v-for="(param, index, i) in differenceItems">
                                    <p v-if="ind == 0" class="mt-8 mb-1 grey--text">{{ param }}</p>
                                    <div class="s-comapre-table-row">
                                        <div>
                                            {{ item.dataParams[param] ? item.dataParams[param] : '–' }}
                                        </div>
                                    </div>
                                    </div>
                                </div>
                                <div v-else-if="!differenceInput" class="s-comapre-table" :class="{'mb40': ind > 0}">
                                    <div v-for="(param, index, i) in dataParameters" >
                                    <p v-if="ind == 0" class="mt-8 mb-1 grey--text">{{ param }}</p>
                                    <div class="s-comapre-table-row">
                                        <div>
                                            {{ item.dataParams[param] ? item.dataParams[param] : '–' }}
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </v-container>
</template>

<script>
import { mapGetters } from 'vuex';
import { uniq } from 'lodash';
export default {
    data() {
        return {
            differenceInput: false,
            allParamInput: true,
            valueList: "Все товары",
            visibleArrItems: [],
            dataFilters: [],
            dataDifFilters: [],
            snackbarCom: false,
            snackbarFav: false,
            snackbarCart: false
        }
    },
    watch: {
        differenceInput(){
         this.allParamInput = !this.differenceInput
        },
        allParamInput(){
        // this.differenceItems?.length == 0
         this.differenceInput = !this.allParamInput
        },
        dataDifFilters(){
        if(this.dataDifFilters?.length == 0){
            this.allParamInput = true
            this.differenceInput = false
        }
        }
    },
    computed: {
        ...mapGetters({
            dataCom: 'compare/compareData',
            countCom: 'compare/countItems',
            dataResultCom: 'compare/dataResult',
            dataResultCart: 'cart/dataResult',
            dataResultFav: 'favorite/dataResult',
            categoriesParent: 'getCategories'
        }),
        hasDifference() {
            if(this.dataDifFilters?.length == 0){
                return false
            } else {
                return true
            }
        },
        hasDistinction: function(){
            return {
                'fa-circle-check': (this.differenceInput) && (this.dataDifFilters.length > 0),
                'fa-circle': (!this.differenceInput) || (this.dataDifFilters.length == 0)
            }
        },
        paramsCom(){
            const arr = ["Все товары"];
            this.dataCom?.map((obj) => {arr.push(obj.category_id);})
            arr.forEach((item, i) => {
                if(i>0){
                    this.categoriesParent.forEach(el => {
                        el.content.forEach(el_sub => {
                            if(el_sub.id == item){
                                    arr[i] = el_sub.name
                            }
                        })
                    })
                }
            })
            return uniq(arr);
        },
        visibleItems(){
            this.visibleArrItems = [];
            if(this.valueList !== "Все товары"){
                this.categoriesParent.forEach(el => {
                        el.content.forEach(el_sub => {
                            if(el_sub.name == this.valueList){
                                this.dataCom.forEach(el => {
                                    if(el.category_id == el_sub.id){
                                        this.visibleArrItems.push(el);
                                    }
                                })
                            }
                        })
                    })
            } 
            else{
                this.visibleArrItems = this.dataCom;
            }
            // let arr = [val];
            return this.visibleArrItems;
        },
        dataParameters(){
            if(this.differenceInput == false && this.visibleArrItems.length !== 0) {
                this.dataFilters = [];
                this.visibleArrItems.forEach(el => {
                    Object.keys(el.dataParams).forEach(key => {
                        if(!this.dataFilters.includes(key)){
                            this.dataFilters.push(key);
                        }
                    })
                })
                this.dataFilters = this.dataFilters.flat();
                this.dataFilters.sort();         
                return this.dataFilters;
            }
        },
        differenceItems(){
            if(this.visibleArrItems.length !== 0) {
                this.dataDifFilters = [];
                 this.dataFilters.forEach(param => {
                    for(let i = 0; i < this.visibleArrItems.length; i++){
                        for(let j = i + 1; j < this.visibleArrItems.length; j++){
                            if(this.visibleArrItems[i].dataParams[param] !== this.visibleArrItems[j].dataParams[param]){
                                if(!this.dataDifFilters.includes(param)){
                                this.dataDifFilters.push(param);
                            }
                                this.dataDifFilters.sort();
                            }
                        }
                    }
                    });
                    return this.dataDifFilters;
                };
            },
        noDifference: function () {
                return{
                    // true: this.dataDifFilters?.length !== 0 && this.valueList == "Все товары",
                    // false: this.dataDifFilters?.length === 0 && this.valueList !== "Все товары",
                    'grey--text': (this.differenceItems?.length == 0),
                    'mb-4': true,
                    'triggerInput': true
                }
            }
    },
    methods: {
        otherLength(param){
            return this.dataCom.filter(el => el.category_id == this.subCategory(param)).length
        },
        subCategory(param){
            let ret;
            if(typeof param == 'number'){
                this.categoriesParent.forEach(el => {
                            el.content.forEach(el_sub => {
                                if(el_sub.id == param){
                                        ret = el_sub.name
                                }
                            })
                        })
            } else{
                this.categoriesParent.forEach(el => {
                            el.content.forEach(el_sub => {
                                if(el_sub.name == param){
                                        ret = el_sub.id
                                }
                            })
                        })
            }
            return ret
        },
        removeItem(val){
            this.snackbarCart = false;
            this.snackbarFav = false;
            this.snackbarCom = val;
        },
        addItemCart(el){
            this.snackbarCom = false;
            this.snackbarFav = false;
            this.snackbarCart = el
        },
        addItemFav(el){
            this.snackbarCart = false;
            this.snackbarCom = false;
            this.snackbarFav = el
        },
        activeAllParams(){
            if(!this.allParamInput) {
                this.allParamInput = true;
                this.differenceInput = false;
            }
        },
        activeDifference(){
            if((!this.differenceInput) && (this.differenceItems.length !== 0)){
                this.differenceInput = true;
                this.allParamInput = false;
            } else {
                this.differenceInput = false;
            }
        },
        removeAll(){
            this.$store.commit('compare/removeAll');
            this.snackbarCom = true;
        }
    },
    async asyncData(params) {
        const title = "Сравнение товаров";
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
        //         image: ["/img/favorite/1.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         dataParams: {
        //             "Ширина": "60 cm",
        //             "Глубина": "55 cm",
        //             "Высота": "10 cm§",
        //             "Габариты2": "60x55",
        //             "Ширина стиральной машины": "60 cm",
        //             "Глубина стиральной машины": "45 cm",
        //             "Вид раковины": "-",
        //             "Форма": "Прямоугольная",
        //             "Расстояние от смесителя до слива": "12 см>",
        //             "Рекомендованная мин. длина излива": "14 см",
        //             "Гарантия": "2 года",
        //             "Страна": "Россия",
        //             "Линии форм": "Прямые",
        //             "Раковина-столешница": "Нет",
        //             "Со скрытым сливом": "Да",
        //         }
        //     },
        //     {
        //         id: 100,
        //         name: "Название товара",
        //         image: ["/img/favorite/3.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         dataParams: {
        //             "Ширина": "60 cm",
        //             "Глубина": "55 cm",
        //             "Высота": "10 cm§",
        //             "Габариты2": "60x55",
        //             "Ширина стиральной машины": "60 cm",
        //             "Глубина стиральной машины": "45 cm",
        //             "Вид раковины": "-",
        //             "Форма": "Прямоугольная",
        //             "Расстояние от смесителя до слива": "12 см>",
        //             "Рекомендованная мин. длина излива": "14 см",
        //             "Гарантия": "2 года",
        //             "Страна": "Россия",
        //             "Линии форм": "Прямые",
        //             "Раковина-столешница": "Нет",
        //             "Со скрытым сливом": "Да",
        //         }
        //     },
        //     {
        //         id: 100,
        //         name: "Название товара",
        //         image: ["/img/favorite/4.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         dataParams: {
        //             "Ширина": "60 cm",
        //             "Глубина": "55 cm",
        //             "Высота": "10 cm§",
        //             "Габариты2": "60x55",
        //             "Ширина стиральной машины": "60 cm",
        //             "Глубина стиральной машины": "45 cm",
        //             "Вид раковины": "-",
        //             "Форма": "Прямоугольная",
        //             "Расстояние от смесителя до слива": "12 см>",
        //             "Рекомендованная мин. длина излива": "14 см",
        //             "Гарантия": "2 года",
        //             "Страна": "Россия",
        //             "Линии форм": "Прямые",
        //             "Раковина-столешница": "Нет",
        //             "Со скрытым сливом": "Да",
        //         }
        //     },
        //     {
        //         id: 100,
        //         name: "Название товара",
        //         image: ["/img/favorite/5.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         dataParams: {
        //             "Ширина": "60 cm",
        //             "Глубина": "55 cm",
        //             "Высота": "10 cm§",
        //             "Габариты2": "60x55",
        //             "Ширина стиральной машины": "60 cm",
        //             "Глубина стиральной машины": "45 cm",
        //             "Вид раковины": "-",
        //             "Форма": "Прямоугольная",
        //             "Расстояние от смесителя до слива": "12 см>",
        //             "Рекомендованная мин. длина излива": "14 см",
        //             "Гарантия": "2 года",
        //             "Страна": "Россия",
        //             "Линии форм": "Прямые",
        //             "Раковина-столешница": "Нет",
        //             "Со скрытым сливом": "Да",
        //         }
        //     },
        //     {
        //         id: 100,
        //         name: "Название товара",
        //         image: ["/img/favorite/6.png"],
        //         code: "4554545",
        //         price: 1540,
        //         old_price: 8220,
        //         brend_name: "Название бренда",
        //         size: "44 x 75 x 20",
        //         available: 1,
        //         dataParams: {
        //             "Ширина": "60 cm",
        //             "Глубина": "55 cm",
        //             "Высота": "10 cm§",
        //             "Габариты2": "60x55",
        //             "Ширина стиральной машины": "60 cm",
        //             "Глубина стиральной машины": "45 cm",
        //             "Вид раковины": "-",
        //             "Форма": "Прямоугольная",
        //             "Расстояние от смесителя до слива": "12 см>",
        //             "Рекомендованная мин. длина излива": "14 см",
        //             "Гарантия": "2 года",
        //             "Страна": "Россия",
        //             "Линии форм": "Прямые",
        //             "Раковина-столешница": "Нет",
        //             "Со скрытым сливом": "Да",
        //         }
        //     }];
        return { title, breadcrumbsData }
    }
}
</script>

<style lang="scss">
.s-catalog-links-common{
    border-bottom: 1px solid #dbdbdb;
}
@media screen and (max-width: 600px) {
    .s-catalog-links-compare{
        .s-catalog-links-el{
            width: unset !important;
        }
    }
}
.mb40{
    >div:first-child>.s-comapre-table-row{
        margin-top: 68px;
    }
    >div:not(:first-child)>.s-comapre-table-row{
        margin-top: 60px;
    }
}
.s-comapre-table {
    .s-comapre-table-row {
        display: flex;
        align-items: center;
        height: 51px;
        padding: 16px 24px;
        &:nth-of-type(odd){
            background: #F6F6F6;
        }
    }
 }
 .checkboxCompare input[type="checkbox"]{
    width: 20px;
    height: 20px;
    border-radius: 50%;
    -webkit-appearance: none;
    background-color: #fff;
    border: 1px solid #ccc;
 }
 .triggerInput:hover{
    cursor: pointer;
}
</style>