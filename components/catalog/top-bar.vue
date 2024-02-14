<template>
    <div class="d-flex justify-space-between top-bar mb-3">
        <v-row class="d-flex align-center justify-space-between w-100">
            <v-col class="pr-2 pl-0 py-0 col-5 col-sm-4 h-100">
                <v-btn @click="$emit('filter-small', $event)" dark class="s-btn-cart s-btn-text w-100">
                    <i class="fa-solid fa-filter mr-1"></i>
                    Фильтр товаров
                </v-btn>
            </v-col>
            <v-col class="d-none d-sm-block px-2 py-0" cols="4">
                <v-select dense outlined v-model="data.sort1" :items="sort1" class="mr-3 w-100" />
            </v-col>
            <v-col class="d-none d-sm-block pl-2 pr-0 py-0" cols="4">
                <v-select dense outlined :value="sort" @input="onInputSort($event)" :items="sortOrder" class="mr-3 w-100" />
            </v-col>
            <v-col class="d-block d-sm-none pa-0 col-5 col-sm-4">
                <!-- <v-menu offset-y :close-on-content-click="false">
                    <template v-slot:activator="{ on, attrs }">
                        <v-btn class="s-btn-cart s-btn-text w-100" plain v-bind="attrs" v-on="on">
                            <i class="fa-solid fa-sort mr-3"></i>

                            Сначала популярные
                        </v-btn>
                    </template>
                    <v-list>
                      <v-list-item>
                        <v-select dense outlined :value="sort" @input="onInputSort($event)" :items="sort1.concat(sortOrder)" class="mr-3 w-100" />
                      </v-list-item>
                    </v-list>
                  </v-menu> -->
                  <!-- <div class="d-flex flex-row align-center"> -->
                      <v-select dense outlined :value="sort" @input="onInputSort($event)" :items="sort1.concat(sortOrder)" class="mr-3 w-100 s-small-select" />
                  <!-- </div> -->
            </v-col>
        </v-row>
        <div class="d-none d-md-block">
            <span>Всего: </span>
            <b>{{ countString }}</b>
        </div>
    </div>
</template>

<script>
import { computed } from 'vue';

export default {
    props: {
        count: Number,
        sort: Object
    },
    data() {
        return {
            data: {
                sort1: 'Популярные',
                sortOrder: { key: "price", order: "ASC" }
            },
            sort1: [
                'Популярные', "Другие"
            ],
            sortOrder: [
                {
                    text: "По возрастанию цены",
                    value: { key: "price", order: "ASC" }
                },
                {
                    text: "По убыванию цены",
                    value: { key: "price", order: "DESC" }
                }
            ],
        }
    },
    watch: {
        "data.sortOrder": {
            handler(v) {
                
            }
        }
    },
    computed: {
        countString() {
            const lastDig = this.count % 10;
            const last2Dig = this.count % 100;
            let w = "товаров";
            if (lastDig == 1) {
                w = "товар";
            } else if (lastDig > 1 && lastDig < 5) {
                w = "товара";
            } else {
                w = "товаров";
            }
            return this.count.toLocaleString('ru') + " " + w
        }
    },
    methods: {
        onInputSort(e) {
            if(e.key){
                this.$router.push({ query: Object.assign({}, this.$route.query, { sort: JSON.stringify(e), page: 0 }) });
            }
        }
    }
}
</script>

<style lang="scss">
    .top-bar{
        .s-btn-cart{
            span{
                text-transform: none !important;
                letter-spacing: normal;
            }
        }
        .v-text-field__details{
            display: none !important;
        }
    }
    .s-small-select{
        min-width: 130px;
        .v-select__selection {
            font-size: 14px !important;
        }
    }
@media screen and (min-width: 426px) and (max-width: 542px) {
    .top-bar{
        .s-btn-cart{
            span{
                font-size: 12px !important;
            }
        }
        .s-small-select{
            .v-select__selection {
                font-size: 12px !important;
            }
        }
    }
}
@media screen and (max-width: 425px) {
    .top-bar{
        .s-btn-cart{
            span{
                font-size: 10px !important;
            }
        }
    }
    .s-small-select{
        .v-select__selection {
            font-size: 10px !important;
        }
    }
}
</style>