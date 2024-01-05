<template>
    <div>
        <v-container class="mb-10">
            <v-divider class="mb-8" />
            <common-beadcrumbs :value="breadcrumbsData" />
            <v-row class="mt-2 d-flex flex-column">
                <v-col cols="2" class="s-catalog-links d-none">
                    <nuxt-link v-for="(el, i) in leftMenu" :key="i" class="s-catalog-links-el" :class="{
                        active: el.url == activeEl.url
                    }" :to="el.url"> <b>{{ el.title }}</b>
                    </nuxt-link>
                </v-col>
                    <v-col cols="12" class="w-100 flex-column px-0 d-flex" id="s-catalog-links-common-parent">
                        <div class="d-flex w-100 justify-space-between s-catalog-links-common">
                            <nuxt-link v-for="(el, i) in leftMenu" :key="i"
                            class="s-catalog-links-el"
                            :class="{active: el.url == activeEl.url}" :to="el.url">
                            {{ el.title }}
                            </nuxt-link>
                        </div>
                    <div class="s-catalog-hr w-100 ma-0 pa-0"></div>
                    </v-col>
                <v-col cols="12" class="px-0">
                    <component :is="'pages-' + activeEl.component" />
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
export default {
    data() {
        return {
        }
    },
    async asyncData(params) {
        console.log(params);
        const leftMenu = [
            {
                title: "О компании",
                url: "/pages/about",
                component: "about"
            },
            {
                title: "Контакты",
                url: "/pages/contacts",
                component: "contacts"
            },
            {
                title: "Доставка",
                url: "/pages/delivery",
                component: "delivery"
            },
            {
                title: "Оплата",
                url: "/pages/payment",
                component: "payment"
            },
            {
                title: "Гарантия и возвраты",
                url: "/pages/garanty",
                component: "garanty"
            }
        ]

        const activeEl = leftMenu.find(el => el.url == params.route.path);
        const breadcrumbsData = [
            {
                url: "",
                title: activeEl.title,
            }
        ];
        const data = [
            {
                title: "О компании",
                url: "/pages/about",
                content: `
                <h1>О компании</h1>
                `
            }
        ]
        return { data, breadcrumbsData, leftMenu, activeEl }
    }
}
</script>

<style lang="scss">
    @media screen and (max-width: 770px) {
        .s-pages-main{
            .s-about-img{
                position: absolute;
                .s-btn-text{
                    position: relative;
                    top: -250px;
                    max-width: 200px;
                    span{
                        font-size: 11px !important;
                    }
                }
            }
        }
    }
    @media screen and (max-width: 723px) {
        .s-pages-main{
            .s-about-img{
                .s-btn-text{
                    top: -230px;
                }
            }
        }
    }
    @media screen and (max-width: 667px) {
        .s-catalog-links-common{
            .s-catalog-links-el{
                

            }
        }
    }
    @media screen and (max-width: 653px) {
        .s-pages-main{
            .s-about-img{
                position: unset;
                .s-btn-text{
                    position: unset;
                    top: 0px;
                    max-width: unset;
                    span{
                        font-size: 12px !important;
                    }
                }
            }
        }
    }
    @media screen and (max-width: 610px) {
        #s-catalog-links-common-parent{
            display: none !important;
        }   
    }
    @media screen and (max-width: 600px) {
        .s-pages-main{
            h1{
                font-size: 36px !important;
            }
            .s-about-images{
                max-width: 400px !important;
                margin: 0 auto;
            }
        }
        .s-pages-row{
            >div:last-child{
                >div{
                    width: 100% !important;
                    >div{
                        margin: 0 auto !important;
                    }
                }
            }
        }   
    }
    @media screen and (max-width: 578px) {
        .s-pages-main{
            .s-contacts-photos{
                >div>div{
                    width: 100px;
                }
            }
        }  
    }
    @media screen and (max-width: 459px) {
        .s-pages-main{
            h1{
                font-size: 30px !important;
                padding-bottom: 10px !important;
            }
        }  
    }
    @media screen and (max-width: 434px) {
        .s-pages-main{
            .s-btn-text{
                span{
                    font-size: 11px !important;
                }
            }
            .s-contacts-col{
                justify-content: space-between;
                a{
                    b{
                        font-size: 14px !important;
                    }
                }
                >a:first-child{
                    margin-right: 0 !important;
                }
            }
            .s-contacts-photo_title{
                font-size: 25px !important;
            }
            .s-contacts-photos{
                >div>div{
                    width: 80px;
                    margin: 10px !important;
                }
            }
        }
    }
    @media screen and (max-width: 425px) {
        .s-contacts-phones{
            >div{
                >div{
                    p{
                        font-size: 14px !important;
                    }
                }
            }
        }
    }

</style>