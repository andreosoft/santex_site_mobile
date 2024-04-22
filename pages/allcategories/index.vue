<template>
    <v-container class="mb-10">
        <v-divider class="mb-8" />
        <common-beadcrumbs class="mb-4" :value="breadcrumbsData" />
        <div class="s-static-main">
            <h1>Каталог</h1>
        </div>
        <v-row class="s-row s-perpage">
            <v-col class="col-md-4 col-sm-6 col-12" v-for="(el, i) in allCategories" :key="i">
              <s-guide-style-items-el :el="el" />
            </v-col>
          </v-row>
      </v-container>
</template>

<script>
import BaseCatalog from "@/components/catalog/base-catalog.vue";
import { mapGetters } from "vuex";
async function getData({route, $axios, $config}){

//   const category_id = route.params.id;
  let breadcrumbsData = [
      {
        url: "/allcategories",
        title: "Каталог",
      }]

  const loading = false
  return { breadcrumbsData, loading};
}

export default {
    components: {
        BaseCatalog
    },
    data () {
        return {
            loading: true
        }
    },
    computed: {
        ...mapGetters ({allCategories: 'getCategories'}),
    },
    async asyncData({ route, $axios, $config }) {
        return await getData({ route, $axios, $config });
  },
}
</script>