<template>
    <div class="s-consulting-main">
        <v-container class="mb-10 s-static-main">
            <s-header title="Запись на консультацию" :breadcrumbsData="breadcrumbsData" />
            <div>
                <div class="my-8">
                    <div>
                        <v-row class="align-center">
                            <v-col cols="12">
                                <v-img :src="'/img/consulting/1.png'" />
                            </v-col>
                            <v-col cols="12">
                                <div class="text-center mx-16">
                                    <h2>Получи помощь<br> по комплектации санузла</h2>
                                    <hr class="style-1" />
                                    <div class="mt-11 s-cons-text">
                                        <p>Наши эксперты салона SantehKomfort Elite могут помочь вам
                                            выбрать качественную сантехнику, которая прослужит
                                            долгие годы. SantehKomfort Elite — лидер по комплектации
                                            ванных комнат. Наши специалисты подберут вам все от скрытых
                                            частей до крючков для полотенец. Также керамическую плитку,
                                            батареи, фильтры для водоснабжения</p>
                                    </div>
                                </div>
                            </v-col>
                        </v-row>
                    </div>
                </div>
            </div>
        </v-container>
        <div style="background: #f2f2f2; margin: 0; padding: 40px 0px;">
            <v-container class="mb-10">
                <div>
                    <div>
                        <div>
                            <v-row class="align-center">
                                <v-col cols="12">
                                    <div class="text-center mx-16">
                                        <h2 class="my-4">Запишись на консультацию</h2>
                                        <hr class="style-1" />
                                        <div class="s-cons-text">
                                            <p>Забронируйте время и дату прихода в наш салон, выбрав
                                                менеджера или запишитесь на консультацию он-лайн.</p>
                                        </div>
                                        <div class="mt-4 d-flex justify-space-around s-cons-btn">
                                            <v-btn class="s-btn-text" style="background-color: transparent;" @click="showPopupConsultShowroom = true">Консультация в салоне</v-btn>
                                            <v-btn class="s-btn-text pa-4" style="background-color: transparent;" @click="showPopupConsult = true">Консультация онлайн</v-btn>
                                        </div>
                                        <s-popup-consult
                                        v-model="showPopupConsult"
                                        @closePopUp="showPopupConsult = false" 
                                        :dataManager="dataShowroom"
                                        >
                                        </s-popup-consult>
                                        <s-popup-consult-showroom 
                                        v-model="showPopupConsultShowroom" 
                                        @closePopUp="showPopupConsultShowroom = false" 
                                        :dataManager="dataShowroom"
                                        >
                                        </s-popup-consult-showroom>
                                    </div>
                                </v-col>
                                <v-col cols="12">
                                    <v-img :src="'/img/consulting/2.png'" />
                                </v-col>
                            </v-row>
                        </div>
                    </div>
                </div>
            </v-container>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            showPopupConsult: false,
            showPopupConsultShowroom: false,
        }
    },
    async asyncData({$config, $axios}) {
        let dataShowroomResponse;
        const breadcrumbsData = [
            {
                url: "",
                title: "Запись на консультацию",
            }
        ];
        try {
            dataShowroomResponse = (await $axios.$get($config.baseURL + "/api/site/content/", 
                {
                params: {
                    filters: {parent_id: 30, status: 1},
                    sort: {"key": "id", "order": "ASC"}
                }
            })).data;
        } catch (error) {
            console.error(error)
        }
        let dataShowroom = [];
        dataShowroomResponse.forEach(el => {dataShowroom.push(el.name)})
        return { breadcrumbsData, dataShowroom }
    }
}
</script>

<style lang="scss">
@media screen and (max-width: 684px) {
    .s-consulting-main{
        div.text-center>h2{
            font-size: 30px !important;
            margin-top: 0 !important;
            margin-bottom: 0 !important;
        }
        .s-cons-text>p{
            font-size: 14px !important;
        }
    }
    .s-cons-btn{
        span{
            font-size: 12px !important;
        }
    }
}
@media screen and (max-width: 633px) {
    .s-cons-btn{
        span{
            font-size: 11px !important;
        }
    }
}

@media screen and (max-width: 600px) {
    hr.style-1{
        margin: 0 auto;
        margin-bottom: 20px;
    }
    .s-consulting-main{
        div.text-center>h2{
            font-size: 25px !important;
        }
        .s-cons-text>p{
            font-size: 14px !important;
        }
    }
    .s-cons-btn{
        flex-direction: column;
        align-items: center;
        >button:first-child{
            margin-bottom: 20px;
        }
        button{
            width: 100% ;
        }
    }
}
@media screen and (max-width: 496px) {
    .s-consulting-main{
        div.text-center{
            margin: auto 20px !important;
        }
        div.text-center>h2{
            font-size: 20px !important;
        }
        .s-cons-text>p{
            font-size: 14px !important;
        }
    }
}
</style>