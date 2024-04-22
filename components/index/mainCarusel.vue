<template>
  <div>
    <v-container class="s-main-slider-container">
      <section id="main-splide" class="splide s-main-slider">
        <div class="splide__track">
          <ul class="splide__list">
            <li v-for="(el, i) in items" :key="i" class="splide__slide">
              <nuxt-link v-if="el.images && el.images[0]" :to="el.params?.url ?? ''"><img :src="$config.baseImageURL + el.images[0]" alt=""></nuxt-link>
            </li>
          </ul>
        </div>
      </section>
    </v-container>
  </div>
</template>

<script>
import { Splide } from '@splidejs/splide';

export default {
  props: {
    items: Array,
  },
  data() {
    return {
      carouselModel: 0,
    };
  },
  computed: {
    length() {
      return this.items.length;
    },
  },
  mounted() {
    var splide = new Splide('#main-splide', {
      perPage: 1,
      // height: 200,
      // fixedWidth: "1370px",
      rewind: true,
      type: 'loop',
      pagination: true
    });
    splide.mount();
  },
  methods: {
    next() {
      this.carouselModel =
        this.carouselModel + 1 === this.length ? 0 : this.carouselModel + 1;
    },
    prev() {
      this.carouselModel =
        this.carouselModel - 1 < 0 ? this.length - 1 : this.carouselModel - 1;
    },
  },
};
</script>

<style lang="scss">
.s-main-slider {
  width: 100%;
  overflow: hidden;

  img{
    width: 100%;
  }
  .splide__slide {
    width: 100% !important;
  }

  .splide__track {
    overflow: inherit;
  }
  @media screen and (max-width: 769px) {
    .splide__arrow--prev, .splide__arrow--next {
      display: none !important;
    }
  }
  @media screen and (max-width: 425px) {
    .splide__arrow--prev, .splide__arrow--next {
      width: 30px !important;
      height: 30px !important;
    }
    .splide__arrow--prev {
      left: 1.5em !important;
    }
  
    .splide__arrow--next {
      right: 1.5em !important;
    }
  }
  .splide__arrow {
    width: 50px;
    height: 50px;
    border: 1px solid #cacaca;
    background: #fff;
    box-shadow: 0px 3px 1px -2px rgb(0 0 0 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(0 0 0 / 12%);
  }

  .splide__arrow--prev {
    left: -2em;
  }

  .splide__arrow--next {
    right: -2em;
  }

  @media only screen and (max-width: 1600px) {

    .splide__arrow--prev {
      left: 4em;
    }

    .splide__arrow--next {
      right: 4em;
    }
  }

  .splide__pagination {
    height: 50px;
    bottom: -19px;

    li {
      margin-bottom: 19px;
    }
  }
}

.s-carusel-blog .v-carousel__controls {
  background: none;
}
</style>