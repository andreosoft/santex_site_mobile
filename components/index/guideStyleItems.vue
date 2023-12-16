<template>
  <v-container>
    <div>
      <div class="s-text-h2 text-center mb-5">
        ГИД ПО СТИЛЮ: найди свой идеальный интерьер
      </div>
      <s-guide-style-items :items="items" />
    </div>
  </v-container>
</template>



<script>
export default {
  props: {
    items: Array
  }
};
</script>