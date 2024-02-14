<template>
  <v-container>
    <v-row class="my-3" align="center">
      <v-col cols="12" lg="7" class="px-0">
        <img class="s-title-img s-title-img1" style="width: 800px" src="/img/main-block1.png" />
        <img class="s-title-img s-title-img2" src="/img/main-block1-small.png" />
      </v-col>
      <v-col cols="12" lg="5" class="s-index-text">
        <h1 class="s-title-header text-center pb-0">
          ТОЛЬКО<br />
          В SANTEHKOMFORT Elite:
        </h1>
        <hr class="style-1 pb-4" />
        <p class="s-title-text text-center mb-4">
          Комплектация санузла от грамотных специалистов с опытом работы от
          10-ти лет. Так же у нас вы можете заказать полностью проект помещения
          или только ванной комнаты.
        </p>
        <div class="text-center mt-5">
          <nuxt-link to="/consulting">
            <v-btn class="s-btn-main s-btn-text">ПОДРОБНЕЕ</v-btn>
          </nuxt-link>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<style lang="scss">
.s-index-text hr.style-1{
  margin: -5px auto 10px auto !important;
}
.s-title-header {
  font-size: 40px;
  font-weight: bold;
}

.s-title-text {
  font-size: 18px;
}

@media (min-width: 1264px) {
  .s-title-img {
    margin-left: -45px;
  }
}

.s-title-img2 {
  display: none;
}

@media only screen and (max-width: 1600px) {
  .s-title-img2 {
    display: block;
  }

  .s-title-img {
    margin-left: 0;
    width: 100%;
  }

  .s-title-img1 {
    display: none;
  }
}
@media only screen and (max-width: 425px) {
  .s-title-header {
    font-size: 25px;
  }
  .s-btn-main{
    width: 100%;
  }
}
</style>