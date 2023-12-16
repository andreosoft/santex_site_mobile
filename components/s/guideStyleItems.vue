<template>
  <div>
    <div>
      <common-carusel :items="items" :type="type" :breakpointsNumbs="breakpoints"/>
    </div>
    <div class="text-center mt-sm-14 pt-8">
      <v-btn class="s-btn-main s-btn-text">
        <NuxtLink to="/">ВСЕ ИНТЕРЬЕРЫ</NuxtLink>
      </v-btn>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: Array
  },
  data() {
    return {
      type: 3,
      breakpoints: {
        830: {
          perPage: 2,
        },
        580: {
          perPage: 1,
        }
      }
    };
  },
};
</script>