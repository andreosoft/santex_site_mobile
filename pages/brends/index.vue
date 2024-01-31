<template>
  <v-container class="mb-10">
    <v-divider class="mb-8" />
    <common-beadcrumbs class="mb-4" :value="breadcrumbsData" />
    <h1>Бренды</h1>
    <v-divider class="mb-8" />
    <div class="d-flex flex-column ">
      <v-row class="s-btn-brend-parent d-flex flex-nowrap flex-sm-wrap">
        <div
        v-for="(el, i) in brendLetters" :key="i" 
        class="s-btn-brend-letter d-flex justify-center align-center"
        :class="{ black: i == activeBrend, 'white--text': i == activeBrend }" 
        @click="activeBrend = i">
        {{ el }}
        </div>
      </v-row>
      <div style="padding-left: 8px;" class="mt-4"> 
        <span class="text-uppercase s-btn" :class="{ active: view == 0 }" @click="view = 0">
        <img style="margin: 0 4px 4px 0;" src="/icons/catalog_card.svg" alt="" />
          Плитка</span>
        <span style="border-left: 1px solid #b8b6b6; padding: 8px 10px; margin-left: 20px;" />
        <span class="text-uppercase s-btn" :class="{ active: view == 1 }" @click="view = 1">
        <img style="margin: 0 4px 4px 0;" src="/icons/catalog_list.svg" alt="" />
          Список</span>
      </div>
    </div>
    <v-divider class="mt-8 mb-16" />
    <div>
      <div v-if="view == 0">
        <v-row>
          <v-col class="col-6 col-sm-4 d-flex justify-center align-center pa-4 s-plate-brend" style="" v-for="(el, i) in data" :key="i">
            <nuxt-link style="max-width: 100%;" to="/">
              <v-img contain :src="el.image[0]" />
              <div class="s-plate-brend-btn" style="">
                <v-btn fab class="black" dark><i class="fas fa-long-arrow-right"></i></v-btn>
              </div>
            </nuxt-link>
          </v-col>
        </v-row>
      </div>
      <div v-else>
        <v-row>
          <v-col class="mb-4 pa-0 col-6 col-sm-4" v-for="(el, i) in data" :key="i">
            <nuxt-link to="/">{{ el.name }}</nuxt-link>
          </v-col>
        </v-row>
      </div>
    </div>
    <div class="">
      <div v-if="view == 0" class="mt-16 text-center">
        <a class="s-btn-else"><i class="fas fa-redo"></i> Показать еще</a>
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      view: 0,
      activeBrend: 10
    }
  },
  async asyncData(params) {
    const brendLetters = [
      'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P',
      'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'
    ]
    const breadcrumbsData = [
      {
        url: "",
        title: "Бренды",
      }
    ];
    const data = [
      {
        name: 'Agape',
        image: ['/img/brends/1.png']
      },
      {
        name: "ARTCERAM",
        image: ["/img/brends/2.png"]
      },
      {
        name: "Axor",
        image: ["/img/brends/3.png"]
      },
      {
        name: "Bette",
        image: ["/img/brends/4.png"]
      },
      {
        name: "Burgbad",
        image: ["/img/brends/5.png"]
      },
      {
        name: "BWT",
        image: ["/img/brends/6.png"]
      },
      {
        name: "Cisal",
        image: ["/img/brends/7.png"]
      },
      {
        name: "Devon&Devon",
        image: ["/img/brends/8.png"]
      },
      {
        name: "Dorn Bracht",
        image: ["/img/brends/9.png"]
      },
      {
        name: "Duravit",
        image: ["/img/brends/10.png"]
      },
      {
        name: "EMCO BAD",
        image: ["/img/brends/11.png"]
      },
      {
        name: "Geberit",
        image: ["/img/brends/12.png"]
      },
      {
        name: "Gessi",
        image: ["/img/brends/13.png"]
      },
      {
        name: "Globo",
        image: ["/img/brends/12.png"]
      },
      {
        name: "Hansgrohe",
        image: ["/img/brends/10.png"]
      },
      {
        name: "Jacuzzi",
        image: ["/img/brends/9.png"]
      },
    ]
    return { data, breadcrumbsData, brendLetters }
  }
}
</script>

<style lang="scss">
@media screen and (max-width: 600px) {
  .s-btn-brend-parent{
    overflow: auto !important;
  }
}
.s-plate-brend {
  margin: 0 -1px -1px 0;
  width: 335px;
  height: 335px;
  border: 1px solid #9e9e9e;
  position: relative;
  .s-plate-brend-btn {
    position: absolute; bottom: 40px; left: 0; right: 0; text-align: center; opacity: 0;
  }
  &:hover {
    background: #ececec;
    .s-plate-brend-btn {
      transition: all 1s;
      opacity: 1;
    }
  }
  transition: all 1s;
}

.s-btn-brend-letter {
  min-width: 36px;
  min-height: 36px;
  border-radius: 50%;
  color: #0000008a;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  padding: 8px;

  &:hover {
    background: #00000018;
  }
}

.s-btn {
  cursor: pointer;
  color: #9e9e9e;
  img {
      filter: invert(60%);
    }

  &.active {
    color: #000;
    img {
      filter: invert(0%);
    }
  }
}
</style>