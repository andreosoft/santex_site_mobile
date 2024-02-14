<template>
  <div>
    <div v-if="value" class="d-flex align-center">
      <i class="fas fa-check-circle mr-2 mb-1"></i>
      <span class="grey--text" style="font-size: 14px">Есть в наличии</span>
    </div>
    <div v-else class="d-flex align-center">
      <i class="fas fa-times-circle mr-2 mb-1"></i>
      <span class="grey--text" style="font-size: 14px">Нет в наличии</span>
    </div>
  </div>
</template>

<script>
export default {
    props: {
        value: Number
    }
}
</script>