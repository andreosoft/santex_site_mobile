<template>
    <v-container class="mb-10 s-designers-main">
        <s-header class="s-static-main" title="Наши дизайнеры" :breadcrumbsData="breadcrumbsData" />
        <h3>Здесь представлены наши дизайнеры и архитекторы, с которыми
            мы реализовали много проектов. </h3>
        <h4 class="mb-16">Вы можете с ними познакомиться и заказать дизайн проект.</h4>
        <div>
            <div class="my-8" v-for="(el, i) in data" :key="i">
                <div v-if="!(i % 2)">
                    <v-row>
                        <v-col cols="12" sm="5" class="px-0 pt-0 pa-sm-0">
                        <v-img class="h-100" :src="$config.baseImageURL + el.images[0]" />
                        </v-col>
                        <v-col cols="12" sm="7" class="pa-0"> 
                            <div class="pl-sm-6">
                                <v-divider class="d-none d-sm-block" />
                                <div>
                                    <h2 class="my-4 pt-10 text-uppercase">{{ el.name }}</h2>
                                    <div class="mb-6" v-html="el.content"></div>
                                    <div class="w-100">
                                        <v-btn class="s-btn-main s-btn-text w-100" :href="el.params ? el.params.url : ''">Перейти на сайт дизайнера</v-btn>
                                    </div>
                                </div>
                            </div>
                        </v-col>
                    </v-row>
                </div>
                <div v-else>
                    <v-row>
                        <v-col cols="12" sm="7">
                            <div class="pr-sm-6">
                                <v-divider class="d-none d-sm-block" />
                                <div>
                                    <h2 class="my-4 pt-10 text-uppercase">{{ el.name }}</h2>
                                    <div class="mb-6" v-html="el.content"></div>
                                    <div>
                                        <v-btn class="s-btn-main s-btn-text w-100" :href="el.params ? el.params.url : ''">Перейти на сайт дизайнера</v-btn>
                                    </div>
                                </div>
                            </div>
                        </v-col>
                        <v-col cols="12" sm="5" class="order-first order-sm-last px-0 pt-0">
                            <v-img class="h-100" :src="$config.baseImageURL + el.images[0]" />
                        </v-col>
                    </v-row>
                </div>
            </div>
        </div>
        <div>
            <div class="mt-16 text-center">
                <a @click="data.length == 4 ?  data = dataDesigners.slice(0, dataDesigners.length) : data = dataDesigners.slice(0, 4)" class="s-btn-else"><i class="fas fa-redo"></i> {{ data.length == 4 ? 'Показать все' : 'Скрыть' }}</a>
            </div>
        </div>
    </v-container>
</template>

<script>
export default {
    async asyncData({$config, $axios}) {
        const breadcrumbsData = [
            {
                url: "",
                title: "Наши дизайнеры",
            }
        ];
        let dataDesigners, data;
        try {
            dataDesigners = (await $axios.get($config.baseURL + '/api/site/content', {
                params: {
                    filters: { parent_id: 43, status: 1 },
                }
            })).data.data;
            data = dataDesigners.slice(0, 4);
            console.log(data.length);
        } catch (error) {
            console.error(error)
        }
        return { breadcrumbsData, data, dataDesigners }
    }
}
</script>

<style lang="scss">
.s-designers-main{
    .s-header-divider{
        display: none !important;
    }
}
@media screen and (min-width: 600px){
    .s-designers-main{
        .s-btn-text{
            width: unset !important;
        }
    }
}
@media screen and (max-width: 768px){
    .s-designers-main{
        h3{
            font-size: 28px;
        }
    }
}
@media screen and (max-width: 700px){
    .s-designers-main{
        .s-btn-text{
            span{
                font-size: 12px;
            }
        }
    }
}
@media screen and (max-width: 640px){
    .s-designers-main{
        .s-btn-text{
            span{
                font-size: 10px;
            }
        }
    }
}

@media screen and (max-width: 600px){
    .s-designers-main{
        h3{
            font-size: 25px;
        }
    }
}
@media screen and (max-width: 599px){
    .s-designers-main{
        .s-btn-text{
            span{
                font-size: 14px;
            }
        }
    }
}

@media screen and (max-width: 425px){
    .s-designers-main{
        h3{
            font-size: 20px;
        }
        h4{
            font-size: 16px;
        }
    }
}
</style>