<template>
  <div class="s-footer white--text py-2 pt-10" style="background: #2a2726;">
    <v-container>
      <v-row class="py-10 d-flex justify-between align-center">
        <v-col class="d-none d-md-flex d-lg-flex" md="2">
          <nuxt-link to="/" ><img style="width: 90%" src="/logo1.png" alt="" /></nuxt-link>
        </v-col>
        <v-col sm="3" md="2" lg="3" class="col-6">
          <div
            class="mb-3 links-foot"
            v-for="(el, i) in leftLinks"
            :key="i"
            style="font-size: 22px"
          >
            <nuxt-link :to="el.to">{{ el.title }}</nuxt-link>
          </div>
        </v-col>
        <v-col sm="4" md="3" lg="3" class="col-6">
          <div
            class="mb-3 links-foot"
            v-for="(el, i) in rightLinks"
            :key="i"
            style="font-size: 22px"
          >
            <nuxt-link :to="el.to">{{ el.title }}</nuxt-link>
          </div>
        </v-col>
        <v-col sm="5" md="auto" lg="3" class="d-flex flex-row flex-sm-column justify-space-between">
          <v-col class="pa-0 pb-sm-8 col-6 col-sm-auto">
            <div style="color: #777" class="text-sm-righttext-md-right name-num-foot">
              Контактный телефон
            </div>
            <div class="num-foot">
              <a style="font-size: 32px; font-weight: lighter" href="tel:84952256285">8 495 225 62 85</a>
            </div>
          </v-col>
          <v-col class="col-6 col-sm-auto parent-btn-foot">
            <v-btn 
            style="text-transform: none; width: 80%;" 
            rounded 
            class="btn-foot black--text s-btn-main s-btn-text" 
            to="/consulting">
            Запись <br class="d-none" id="s-foot_br"> на консультацию
            </v-btn>
          </v-col>
        </v-col>
      </v-row>
      <hr />
      <v-row class="s-bottom">
        <v-col style="text-align: center !important;" cols="6" lg="2"><a href="/">© SantehKomfort Elite, {{ new Date().getFullYear() }}</a> </v-col>
        <v-col style="text-align: center !important;" cols="6" lg="2"><nuxt-link to="/publicOffer">Публичная оферта</nuxt-link> </v-col>
        <v-col style="text-align: center !important;" cols="6" lg="2"><nuxt-link to="/privacy">Политика конфиденциальности</nuxt-link></v-col>
        <v-col style="text-align: center !important;" cols="6" lg="2"><nuxt-link to="/acceptRules">Правила приемки</nuxt-link> </v-col>
        <v-col style="text-align: center !important;" cols="6" lg="2"><nuxt-link to="/userAgreement">Пользовательское соглашение</nuxt-link></v-col>
        <v-col cols="6" xs="6" lg="2" class="text-right text-sm-center order-2 mench-footer">
          <img style="width: 90%" src="/img/mench1.png" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showPopupConsult: false,
      leftLinks: [
        {
          to: "/",
          title: "Каталог",
        },
        {
          to: "/",
          title: "Распродажа",
        },
        {
          to: "/brends",
          title: "Бренды",
        },
        {
          to: "/blog",
          title: "Блог",
        },
        {
          to: "/designers",
          title: "Дизайнеры",
        },
      ],
      rightLinks: [
        {
          to: "/pages/about",
          title: "О компании",
        },
        {
          to: "/pages/contacts",
          title: "Контакты",
        },
        {
          to: "/pages/payment",
          title: "Оплата",
        },
        {
          to: "/pages/delivery",
          title: "Доставка",
        },
        {
          to: "/pages/garanty",
          title: "Гарантия и возвраты",
        },
      ],
    };
  },
};
</script>

<style lang="scss">
.s-footer {
  a {
    color: #eee !important;
  }
  hr {
    border: none;
    border-bottom: solid 2px #777;
    margin: 20px 0;
  }
  .s-bottom {
    a {
      color: #777 !important;
      font-size: 14px;
    }
  }
  
}
@media screen and (max-width: 768px) {
  .btn-foot{
    width: auto;
    font-size: 11px !important;
  }
  .links-foot{
    a{
      font-size: 19px !important;
    }
  }
  .num-foot>a {
    font-size: 26px !important;
  }
  .num-foot{
    text-align: left !important;
  }
  .parent-btn-foot{
    text-align: left !important;
  }
}

@media screen and (max-width: 600px) {
  .num-foot>a {
    font-size: 25px !important;
  }
  .links-foot{
    a{
      font-size: 15px !important;
    }
  }
  .parent-btn-foot{
    padding: 0 !important;
  }
  .btn-foot span{
    text-align: center;
  }
  #s-foot_br{
    display: block !important;
  }
}
@media screen and (max-width: 534px) {
  .name-num-foot{
   font-size: 14px !important; 
  }
  .btn-foot{
    font-size: 10px !important;
  }
  .num-foot>a {
    font-size: 20px !important;
  }
}
@media screen and (max-width: 425px) {
  .btn-foot{
    width: 100% !important;
    text-align: center;
  }
  .s-bottom{
    div{
      text-align: left !important;
      a{
        font-size: 12px !important;
      }
    }
  }
}
@media screen and (max-width: 375px) {
  .btn-foot{
    font-size: 10px !important;
    margin-left: 5px !important;
  }
  .num-foot>a {
    font-size: 20px !important;
  }
  .s-bottom{
    div{
      a{
        font-size: 10px !important;
      }
    }
  }
}




@media screen and (min-width: 600px) {
  .num-foot{
    text-align: right !important;
  }
  .parent-btn-foot{
    padding: 0 !important;
    text-align: right !important;
  }
  .name-num-foot{
    text-align: right !important;
  }
  .mench-footer{
    img{
      width: auto !important;
    }
  }
}
</style>