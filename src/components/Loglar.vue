<template>
  <div>
    <h3>LOGLAR</h3>

    <p v-if="props.olaylar.length > 0" v-for="olay in props.olaylar">
      {{ olay.kim }} {{ olay.kime }} {{ olay.kaçVurdu }} vurdu
    </p>
  </div>
</template>

<script setup>
var props = defineProps({
  olaylar: {
    type: Array,
    required: true,
  },
})
</script>

<style lang="scss" scoped></style>
