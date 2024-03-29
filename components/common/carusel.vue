<template>
  <div>
    <v-container class="s-common-carusel">
      <section ref="splideEl" class="splide">
        <div class="splide__track">
          <ul class="splide__list">
            <li v-for="(el, i) in items" :key="i" class="splide__slide">
              <common-video v-if="blog" :item="el"/>
              <index-new-items-el-carusel v-else-if="sales" :el="el" :newItems="newItems" :sales="sales" />
              <s-guide-style-items-el v-else :el="el" />
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
    type: {
      type: Number,
      default: 2,
    },
    slides: {
      type: Number,
      default: 3,
    },
    gap: {
      type: String,
      default: '1.5rem',
    },
    breakpointsNumbs: {
      type: Object,
      default: () => ({})
    },
    pager: {
      type: Boolean,
      default: false
    },
    sales: {
      type: Boolean,
      default: false
    },
    newItems: {
      type: Boolean,
      default: false
    },
    blog: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      carouselModel: 0,
    };
  },
  mounted() {
    var splide = new Splide(this.$refs.splideEl, {
      perPage: this.slides,
      gap: '1.5rem',
      // fixedWidth: "1370px",
      rewind: true,
      type: 'loop',
      pagination: this.pager,
      breakpoints: this.breakpointsNumbs,
      // height: this.sales ? 231 : 'auto',
      // width: this.sales ? 351 : 'auto',
    });
    splide.mount();
  },
  computed: {
    // data() {
    //   let r = [];
    //   if (this.items) {
    //     for (let i = 0; i < this.items.length / this.type; i++) {
    //       r[i] = [];
    //       for (let k = 0; k < this.type; k++) {
    //         let a = i * this.type + k;
    //         if (this.items[a]) r[i].push(this.items[a]);
    //       }
    //     }
    //   }
    //   return r;
    // },
    length() {
      return this.items.length / this.type;
    },
  },
  // methods: {
  //   next() {
  //     this.carouselModel =
  //       this.carouselModel + 1 === this.length ? 0 : this.carouselModel + 1;
  //   },
  //   prev() {
  //     this.carouselModel =
  //       this.carouselModel - 1 < 0 ? this.length - 1 : this.carouselModel - 1;
  //   },
  // },
};
</script>

<style lang="scss">
.s-common-carusel {
  .splide__arrow {
    position: absolute;
    top: calc(50%);
    z-index: 1;
    width: 50px;
    height: 50px;
    border: 1px solid #cacaca;
    background: #fff;
    box-shadow: 0px 3px 1px -2px rgb(0 0 0 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(0 0 0 / 12%);


    &.splide__arrow--prev {
      left: -80px;
    }

    &.splide__arrow--next {
      right: -80px;
    }
  }

  @media only screen and (max-width: 1600px) {
    .splide__arrow {
      position: absolute;
      top: calc(50%);
      z-index: 1;

      &.splide__arrow--prev {
        left: 10px;
      }

      &.splide__arrow--next {
        right: 10px;
      }
    }
  }
}

// .s-carusel-blog .v-carousel__controls {
//   background: none;
// }

// .s-carusel-contols {
//   position: absolute;
//   top: calc(40% - 28px);
//   z-index: 1;

//   &.left {
//     left: -80px;
//   }

//   &.right {
//     right: -80px;
//   }

//   @media only screen and (max-width: 1600px) {
//     .v-btn--fab.v-size--default {
//       height: 36px;
//       width: 36px;
//     }

//     &.left {
//       left: 10px;
//     }

//     &.right {
//       right: 10px;
//     }
//   }
// }
</style>