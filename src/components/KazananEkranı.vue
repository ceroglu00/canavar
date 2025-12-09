<template>
  <div class="wrapper">
    <p
      style="
        text-align: center;
        background-color: #f6f279;
        padding: 10px;
        border-radius: 5px;
        color: #680f0f;
      "
    >
      OYUN BİTTİ
    </p>
    <p>
      <span style="font-weight: bold">KAZANAN: </span>
      <span style="margin-left: 5px"> {{ props.kazananKisi }} </span>
    </p>
    <p v-if="props.kazananKisi == 'OYUNCU'">TEBRIKLER KAZANDINIZ</p>
    <p v-else>YAZIKLAR OLSUN YENİLDİN</p>

    <button @click="emit('YenidenBaslat')">YENİDEN BAŞLAT</button>
  </div>
</template>

<script setup>
var props = defineProps({
  kazananKisi: {
    type: String,
    required: true,
  },
  kaybedenKisi: String,
})

var emit = defineEmits(['YenidenBaslat', 'OyunuKapat'])

</script>

<style scoped>
.wrapper {
  background-color: #a3a3a3;
  border: aquamarine 1px solid;
  border-radius: 10px;
  padding: 20px;
  max-width: 400px;
  margin: 25px auto;
  transition: background-color 2s ease;
}
</style>
