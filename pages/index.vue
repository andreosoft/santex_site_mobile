<template>
    <div class="s-static-main s-index-main">
        <index-mainCarusel
            v-if="carouselItems?.length > 0"
            :items="carouselItems"
            class="mb-10"
        />
        <index-mainInfoBlock />
        <common-divider1 />
        <index-new-items v-if="newItems" :items="newItems" class="mb-5" />
        <common-divider1 v-if="newItems" class="mb-10" />
        <index-guide-style-items
            v-if="dataInterior?.length > 0"
            :items="dataInterior"
            class="mb-5"
        />
        <common-divider1 />
        <index-catalog-items
            v-if="catalogItems?.length > 0"
            :items="catalogItems"
            class="mb-5"
        />
        <common-divider1 />
        <index-salesItems
            v-if="salesRes?.length > 0"
            :items="salesItems"
            class="mb-5"
        />
        <common-divider1 v-if="salesRes?.length > 0" />
        <index-video :item="videoItem" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            carouselModel: 0,
        };
    },
    async asyncData({ $axios, $config }) {
        let dataInterior = [];
        try {
            dataInterior = (
                await $axios.get($config.baseURL + "/api/site/interior")
            ).data.data;
        } catch (error) {
            console.error(error);
        }
        let salesItems = [];
        try {
            salesItems = (
                await $axios.get($config.baseURL + "/api/site/promote", {
                    params: { filters: { type: 1, status: 1 } },
                })
            ).data.data;
            // console.log('Товары со скидкой')
            // console.log(salesItems);
        } catch (error) {
            console.error(error);
        }
        // console.log(salesItems);

        let salesRes;
        try {
            salesRes = (
                await $axios.get(
                    $config.baseURL + "/api/site/promote_catalog",
                    {
                        params: {
                            f: {},
                            filters: {
                                status: 1,
                                "ic.promote_id": salesItems[0].id,
                            },
                            sort: { key: "price", order: "ASC" },
                            pager: { page: 0, count: 0, limit: 0 },
                        },
                    },
                )
            ).data.data;
        } catch (e) {
            console.error(e);
        }

        let newItems = [];
        try {
            newItems = (
                await $axios.get($config.baseURL + "/api/site/promote", {
                    params: {
                        filters: { status: 1, type: 2 },
                    },
                })
            ).data.data;
        } catch (error) {
            console.error(error);
        }
        // console.log(newItems);
        let itemsNewRes;
        try {
            itemsNewRes = (
                await $axios.get(
                    $config.baseURL + "/api/site/promote_catalog",
                    {
                        params: {
                            f: {},
                            filters: {
                                status: 1,
                                "ic.promote_id": newItems[0].id,
                            },
                            sort: { key: "price", order: "ASC" },
                            pager: { page: 0, count: 0, limit: 0 },
                        },
                    },
                )
            ).data.data;
        } catch (e) {
            console.error(e);
        }

        const videoItem = {
            img: "/img/coll1.png",
            video: "https://www.youtube.com/embed/nOKam63GgzQ",
        };

        // const carouselItems = [
        //   {
        //     to: "/",
        //     img: "/banners/banner1.png",
        //   },
        //   {
        //     to: "/",
        //     img: "/banners/banner1.png",
        //   },
        //   {
        //     to: "/",
        //     img: "/banners/banner1.png",
        //   },
        //   {
        //     to: "/",
        //     img: "/banners/banner1.png",
        //   },
        // ];
        let catalogItems = [];
        try {
            catalogItems = (
                await $axios.get($config.baseURL + "/api/site/categories", {
                    params: {
                        filters: { status: 1, main_show: 1 },
                        sort: { key: "sort", order: "ASC" },
                    },
                })
            ).data.data;
        } catch (error) {
            console.error(error);
        }
        let carouselItems = [];
        try {
            carouselItems = (
                await $axios.get($config.baseURL + "/api/site/content", {
                    params: {
                        filters: { parent_id: 52 },
                        sort: { key: "sort", order: "ASC" },
                    },
                })
            ).data.data;
        } catch (error) {
            console.error(error);
        }
        // console.log(catalogItems);
        // console.log(carouselItems);
        return {
            carouselItems,
            catalogItems,
            videoItem,
            dataInterior,
            salesItems,
            salesRes,
            itemsNewRes,
            newItems,
        };
    },
};
</script>

<style lang="scss">
.s-index-main {
    .s-divider1-container {
        padding-top: 0 !important;
        .s-common-divider1 {
            margin-top: 0 !important;
        }
    }
}
</style>
