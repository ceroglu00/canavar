<template>
  <div>
    <h3>LOGLAR</h3>

<!--    <p class="logWrapper"-->
<!--       v-if="props.olaylar.length > 0"-->
<!--       v-for="(olay,index) in props.olaylar"-->
<!--    >-->
<!--      {{index}}-->
<!--    {{ olay.kim }} {{ olay.kime }} {{ olay.kaçVurdu }}-->
<!--      <span v-if="olay.aksiyon=='vuruş'">vurdu</span>-->
<!--      <span v-else-if="olay.aksiyon=='canbasma'">can bastı</span>-->
<!--    </p>-->


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

<style scoped>

.logWrapper{
  background-color: aquamarine;
}

</style>
