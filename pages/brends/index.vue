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
          <v-col class="col-6 col-sm-4 d-flex justify-center align-center pa-4 s-plate-brend" style="" v-for="(el, i) in activeData" :key="i">
            <nuxt-link style="max-width: 100%;" :to="'/catalog/search?q=' + el.name">
              <v-img v-if="el.images && el.images[0]" contain :src="$config.baseImageURL + el.images[0]"/>
              <v-img v-else contain :src="'/black-square.jpg'"/>
              <div class="s-plate-brend-btn" style="">
                <v-btn fab class="black" dark><i class="fas fa-long-arrow-right"></i></v-btn>
              </div>
            </nuxt-link>
          </v-col>
        </v-row>
      </div>
      <div v-else>
        <v-row>
          <v-col class="mb-4 pa-0 col-6 col-sm-4" v-for="(el, i) in activeData" :key="i">
            <nuxt-link :to="'/catalog/search?q=' + el.name">{{ el.name }}</nuxt-link>
          </v-col>
        </v-row>
      </div>
    </div>
    <div>
      <div v-if="view == 0 && activeData.length >= 16" class="mt-16 text-center">
        <a @click="toggleAll = !toggleAll" class="s-btn-else"><i class="fas fa-redo"></i> Показать еще</a>
      </div>
    </div>
  </v-container>
</template>

<script>
import { uniq } from 'lodash';
export default {
  data() {
    return {
      view: 0,
      activeBrend: 0,
      toggleAll: false
    }
  },
  computed: {
  activeData() {
    if (this.dataBrands.filter(item => item['name'].trim()[0] == this.brendLetters[this.activeBrend])) {
      if(this.view == 0){
        if(this.toggleAll) {
          return this.dataBrands.filter(item => item['name'].trim()[0] == this.brendLetters[this.activeBrend]);
        } else {
          return this.dataBrands.filter(item => item['name'].trim()[0] == this.brendLetters[this.activeBrend]).slice(0, 16);
        }
      } else {
        return this.dataBrands.filter(item => item['name'].trim()[0] == this.brendLetters[this.activeBrend]);
      }

    }
  }
},
  async asyncData({ $axios, $config }) {
    const breadcrumbsData = [
      {
        url: "",
        title: "Бренды",
      }
    ];
    const dataBrands = (await $axios.get($config.baseURL + '/api/site/brand', { params: { filters: { status: 1 } } })).data.data;

    let brendLetters = [];
    dataBrands.forEach(el => {
      brendLetters.push(el['name'].trim()[0]);
    })

    brendLetters = uniq(brendLetters).sort();
    return { breadcrumbsData, brendLetters, dataBrands }
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
  border-radius: 100px;
  color: #0000008a;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;

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