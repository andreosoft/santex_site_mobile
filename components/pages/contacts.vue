<template>
    <div class="s-pages-main">
        <h1>Контакты</h1>
        <v-row>
            <v-col class="col-12 col-sm-8 px-0">
                <p class="grey--text"><b>АДРЕС</b></p>
                <p style="font-size: 18px;">г. Москва, Волгоградский проспект, д. 32, корпус 25,<br/>
                    ТЦ «Метр квадратный», 2 этаж, пав. № 66-67-96</p>
            </v-col>
            <v-col class="col-12 col-sm-4 px-0 pl-sm-3">
                <p class="grey--text">
                    <b>Режим работы</b>
                </p>
                <p style="font-size: 18px;">
                    Ежедневно с 10:00 до 20:00
                </p>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="col-12 px-0 s-contacts-phones">
                <v-row>
                    <v-col class="pl-0 col-6 col-sm-4">
                        <p class="grey--text"><b>ТЕЛЕФОНЫ</b></p>
                        <h4 class="pb-2" style="font-size: 18px;"><a href="tel://88007759985">8 (800) 775-99-85</a></h4>
                        <p>Звонок по России бесплатный</p>
                    </v-col>
                    <v-col class="pr-0 col-6 col-sm-4 pr-sm-3">
                        <p class="grey--text"><b>САНТЕХНИКА</b></p>
                        <h4 class="pb-2" style="font-size: 18px;"><a href="tel://84952256285">8 (495) 225 62 85</a></h4>
                        <p>Ежедневно с 10:00 до 20:00</p>
                    </v-col>
                    <v-col class="d-none d-sm-block col-4 pr-0">
                        <p class="grey--text"><b>ПЛИТКА</b></p>
                        <h4 class="pb-2" style="font-size: 18px;"><a href="tel://79055368559">+7 (905) 536-85-59</a></h4>
                        <p>Ежедневно с 10:00 до 20:00</p>
                    </v-col>
                </v-row>
            </v-col>
            <v-col class="col-6 col-sm-4 pl-0 s-contacts-plitka">
                <p class="grey--text">
                    <b>E-mail</b>
                </p>
                <h4 style="font-size: 18px;"><a href="mailto:info@santax.ru">info@santax.ru</a></h4>
            </v-col>
            <v-col class="col-6 d-sm-none col-sm-4 pr-0 pl-sm-0 s-contacts-plitka">
                <p class="grey--text"><b>ПЛИТКА</b></p>
                <h4 class="pb-2" style="font-size: 18px;"><a href="tel://79055368559">+7 (905) 536-85-59</a></h4>
                <p>Ежедневно с 10:00 до 20:00</p>
            </v-col>
        </v-row>
        <v-row class="mb-4 s-contacts-col">
            <v-col class="col-12 col-sm-4 pl-0 d-flex d-sm-block s-contacts-col">
                <a href="https://www.instagram.com/santehkomfort?igsh=MTN6OWVmaTNlcHMwcQ==" class="d-flex align-center mb-sm-6 mr-10 mr-sm-0">
                    <div class="pa-1 pr-4"><img src="/icons/contact/inst.png" /></div>
                    <div>
                        <div class="grey--text"><b>Instagram</b></div>
                        <div><b>santehkomfort</b></div>
                    </div>
                </a>
                <a href="https://instagram.com/tanya_prodesign" class="d-flex align-center">
                    <div class="pa-1 pr-4 d-sm-block d-none"><img src="/icons/contact/inst.png" /></div>
                    <div>
                        <div class="grey--text"><b>Instagram</b></div>
                        <div><b>tanya_prodesign</b></div>
                    </div>
                </a>
            </v-col>
            <v-col class="col-12 pl-0 pl-sm-3 col-md-4 col-sm-8 d-flex d-sm-block s-contacts-col">
                <a href="https://api.whatsapp.com/send/?phone=79672823107" class="d-flex align-center mb-sm-6 mr-10 mr-sm-0">
                    <div class="pa-1 pr-4"><img src="/icons/contact/whatsapp.png" /></div>
                    <div>
                        <div class="grey--text" >
                            <b>Сантехника</b>
                        </div>
                        <div><b>+7 (967) 282-31-07</b></div>
                    </div>
                </a>
                <a href="https://api.whatsapp.com/send/?phone=79055368559" class="d-flex align-center">
                    <div class="pa-1 pr-4 d-sm-block d-none"><img src="/icons/contact/whatsapp.png" /></div>
                    <div>
                        <div class="grey--text">
                            <b>Плитка</b>
                        </div>
                        <div><b>+7 (905) 536-85-59</b></div>
                    </div>
                </a>
            </v-col>
            <v-col class="col-md-4 col-12 px-0" >
                <a href="https://youtube.com/@tanyaprodesign" class="d-flex align-center mb-6">
                    <div class="pa-1 pr-4"><img src="/icons/contact/youtube.png" /></div>
                    <div>
                        <div class="grey--text"><b>Youtube</b></div>
                        <div><b>Tanya PROdesign</b></div>
                    </div>
                </a>
            </v-col>
        </v-row>
        <div class="mb-8">
            <v-img src="/img/about/6.png" />
        </div>
        <div class="mb-8">
            <common-video fullWidth :item="videoItem" />
        </div>
        <div class="">
            <div>
                <div class="d-flex mb-4">
                    <div class="mr-6"><i class="fas fa-check-circle"></i></div>
                    <div>В навигаторе «МЕТР КВАДРАТНЫЙ» ПАРКОВКА НА ТЕРРИТОРИИ БЕСПЛАТНАЯ.</div>
                </div>
                <div class="d-flex mb-4">
                    <div class="mr-6"><i class="fas fa-check-circle"></i></div>
                    <div>От метро «Волгоградский проспект», первый вагон из центра, после турникетов налево, идти прямо
                        700 метров.
                        Проходите СБЕРБАНК И ЗА НИМ «ТЦ Метр квадратный». </div>
                </div>
                <div class="d-flex mb-4">
                    <div class="mr-6"><i class="fas fa-check-circle"></i></div>
                    <div>От МЦК «Угрешская» , после турникетов направо в сторону Волгоградского проспекта пройти 400
                        метров до «ТЦ Метр квадратный».
                    </div>
                </div>
                <div class="d-flex mb-4">
                    <div class="mr-6"><i class="fas fa-check-circle"></i></div>
                    <div>Как нас найти в ТЦ «Метр Квадратный»: Центральный вход. В УЗКОМ ПРОЕЗДЕ. (в офисный центр НЕ
                        НАДО) Заходите в СРЕДНЮЮ
                        ДВЕРЬ вход с КРАСНЫМ козырьком, поднимаетесь на 2-й этаж (самый верхний) на лифте или на
                        эскалаторе. С эскалатора направо -
                        линия Ж, наклейки с номерами павильонов на полу Из лифта прямо - линия Ж, наклейки с номерами
                        павильонов на полу. За домом
                        обоев, наш самый большой павильон с сантехникой и плиткой.</div>
                </div>
            </div>
        </div>
        <common-divider1 />
        <div class="mb-8">
            <h2 class="text-center s-contacts-photo_title">Фотографии салона</h2>
            <div>
                <div class="mb-4">
                    <v-img :src="'/img/contact/' + showImage + '.png'" />
                </div>
                <div class="d-flex justify-center s-contacts-photos">
                    <div class="d-flex">
                        <div class="ma-4" style="max-width: 200px">
                            <v-img @click="showImage = 2" src="/img/contact/3.png" />
                        </div>
                        <div class="ma-4" style="max-width: 200px">
                            <v-img @click="showImage = 4" src="/img/contact/4.png" />
                        </div>
                        <div class="ma-4" style="max-width: 200px">
                            <v-img @click="showImage = 5" src="/img/contact/5.png" />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
data() {
    return {
        showImage: 2,
        showPopupConsult: false,
        videoItem: {
                video: "https://www.youtube.com/embed/nOKam63GgzQ"
            },
        }
    },
}
</script>