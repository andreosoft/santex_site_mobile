<template>
  <v-card>
    <NuxtLink :to="el.parent_id > 0 ? '/catalog/' + el.parent_id : '/allcategories/' + el.id">
      <v-img v-if="el.images" :src="$config.baseImageURL + el.images[0]"/>
        <v-img v-else src="/black-square.jpg" />
      <div style="position: absolute; bottom: 30px; left: 0; right: 0;">
        <div class="pa-2 text-center">
          <v-btn class="s-btn-text catalog-text">{{ el.name }}</v-btn>
        </div>
      </div>
    </NuxtLink>
  </v-card>
</template>

<script>
export default {
  props: {
    el: Object,
  },
};
</script>

<style lang="scss">
@media screen and (max-width: 840px) {
  .catalog-text{
    span{
      font-size: 10px !important;
    }
  }
}
@media screen and (max-width: 665px) {
  .catalog-text{
    span{
      font-size: 8px !important;
    }
  }
}
</style>