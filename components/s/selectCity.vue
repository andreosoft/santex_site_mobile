<template>
    <div class="d-inline">
        <span style="cursor: pointer" @click="dialog = true">
            <span>{{ citys[value] }}</span>
            <img src="/icons/arrow_down_city.svg" />
        </span>

        <v-dialog 
        v-model="dialog" 
        scrollable 
        persistent 
        :overlay="false" 
        max-width="730px" 
        transition="dialog-transition"
        :fullscreen="$vuetify.breakpoint.xs"
        style="z-index: 2000;"
        >
            <div class="s-popup">
                <div>
                    <div class="text-right">
                        <v-btn icon @click="dialog = false"><img src="/icons/close_menu.svg" /></v-btn>
                    </div>
                </div>
                <div style="margin: 10px 64px;" class="mb-sm-16 parent-dialog">
                    <div class="d-flex header-info">
                        <h2 class="s-text-city">Выбрать город</h2>
                        <v-spacer class="s-spacer"></v-spacer>
                        <div style="cursor: pointer; margin-top: 6px;" class="grey--text s-text-auto mb-5 mb-sm-0">Определить автоматически</div>
                    </div>
                    <div class="search-city">
                        <v-text-field single-line outlined label="Найти город" background-color="#EFF1F5">
                            <template v-slot:append>
                                <img src="/icons/Search.svg" />
                            </template>
                        </v-text-field>
                    </div>
                    <div>
                        <v-row class="mt-8 dialog-cities d-none d-sm-flex">
                            <v-col cols="4" v-for="(el, i) in citys" :key="i">
                                <a @click="dialog = false; $emit('input', i)" class="underlined-onhover">{{ el }}</a>
                            </v-col>
                        </v-row>
                        <v-row class="mt-8 dialog-cities-small d-sm-none">
                            <v-col cols="12" class="d-flex flex-wrap flex-row">
                                <v-col class="pa-0 pb-4" cols="6" v-for="(el, i) in citys" :key="i">
                                    <a @click="dialog = false; $emit('input', i)" class="underlined-onhover">{{ el }}</a>
                                </v-col>
                            </v-col>
                            <!-- <v-col cols="6" >
                                <v-col class="pa-0 pb-4" cols="12" v-for="(el, i) in Object.values(citys).slice(Object.values(citys).length/2)" :key="i">
                                    <a @click="dialog = false; $emit('input', i)" class="underlined-onhover">{{ el }}</a>
                                </v-col>
                            </v-col> -->
                        </v-row>
                    </div>
                </div>
            </div>
        </v-dialog>
    </div>
</template>

<script>
export default {
    props: {
        value: String,
    },
    data() {
        return {
            dialog: false,
            citys: {
                "moscow": "Москва",
                "ek": "Екатеринбург",
                "kr": "Краснодар",
                "nn": "Нижний Новгород",
                "nov": "Новосибирск",
                "rost": "Ростов-на-Дону",
                "sam": "Самара",
                "spb": "Санкт-Петербург",
                "ufa": "Уфа",
                "chel": "Челябинск"
            },
        }
    }
}
</script>

<style lang="scss">
@media screen and (max-width: 768px) {
    .s-text-city{
        font-size: 25px !important;
    }
    .s-text-auto{
        font-size: 14px !important;
    }
    .dialog-cities{
        div{
            a{
                font-size: 14px !important;
            }
        }
    }
}
@media screen and (max-width: 600px) {
    .header-info{
        flex-direction: column;
    }
    .s-spacer{
        display: none;
    }
    .s-text-city{
        font-size: 25px !important;
    }
    .s-text-auto{
        font-size: 14px !important;
    }
    .dialog-cities-small{
        padding-right: 10px !important;
        div{
            padding: 0 !important;
            padding-bottom: 20px !important;
            a{
                font-size: 14px !important;
            }
        }
    }
    .header-info{
        h2{
            padding-bottom: 10px !important;
        }
    }
}
@media screen and (max-width: 425px) {
    .parent-dialog{
        margin: 10px 40px !important;
        margin-bottom: 20px !important;
    }
    .s-text-city{
        font-size: 25px !important;
    }
    .s-text-auto{
        font-size: 14px !important;
    }
    .dialog-cities-small{
        div{
            a{
                font-size: 14px !important;
            }
        }
    }
}
@media screen and (max-width: 375px) {
    .s-text-city{
        font-size: 22px !important;
    }
    .s-text-auto{
        font-size: 12px !important;
    }
    .dialog-cities-small{
        padding-right: 20px !important;
        div{
            a{
                font-size: 12px !important;
            }
        }
    }
}

</style>