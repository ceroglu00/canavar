<template>
  <h1 class="text-center" style="font-weight: bold">CENAVAR</h1>

  <section v-if="canavar.can > 0 && oyuncu.can > 0" name="OYUN">
    <section name="OYUNCU">
      <p>Oyuncu</p>
      <div
        class="canBarı"
        :class="[
          oyuncu.can <= 9 ? 'siyahYazı' : '',
          oyuncu.can < 30 ? 'barRenkLOW' : '',
          oyuncu.can >= 30 && oyuncu.can < 60 ? 'barRenkDAMAGED' : '',
          oyuncu.can >= 60 && oyuncu.can < 80 ? 'barRenkMEDIUM' : '',
          oyuncu.can >= 80 ? 'barRenkFULL' : '',
        ]"
        :style="`width: ${PixelHesapla(200, oyuncu.can)}px`"
      >
        {{ oyuncu.can }}
      </div>
    </section>

    <section name="CANAVAR" style="margin-top: 30px">
      <p>Canavar</p>
      <div
        class="canBarı"
        :class="[
          canavar.can <= 9 ? 'siyahYazı' : '',
          canavar.can < 30 ? 'barRenkLOW' : '',
          canavar.can >= 30 && canavar.can < 60 ? 'barRenkDAMAGED' : '',
          canavar.can >= 60 && canavar.can < 80 ? 'barRenkMEDIUM' : '',
          canavar.can >= 80 ? 'barRenkFULL' : '',
        ]"
        :style="`width: ${PixelHesapla(200, canavar.can)}px`"
      >
        {{ canavar.can }}
      </div>
    </section>

    <section name="AKSİYONLAR" style="margin-top: 30px">
      <button @click="Atak()">ATAK</button>
      <button :disabled="!oyuncu.özelAtakHakkı" @click="OzelAtak">ÖZEL ATAK</button>
      <button @click="CanBas">CAN BAS</button>
    </section>
  </section>
  <section v-else name="KAZANAN EKRANI">
    <KazananEkranı @YenidenBaslat="Restart" :kazananKisi="kazanan"></KazananEkranı>
  </section>

  <section name="LOGLAR">
    <Loglar :olaylar="olaylar"></Loglar>
    <p v-if="olaylar.length == 0">LOG BULUNAMADI</p>
  </section>
</template>

<script setup>
// PARENT to CHILD
// CHILD to PARENT

import { computed, reactive, ref, watch } from 'vue'
import KazananEkranı from '@/components/KazananEkranı.vue'
import Loglar from '@/components/Loglar.vue'

var olaylar = reactive([])

var kazanan = computed(() => {
  console.log('COMPUTED ÇALIŞTI')
  if (canavar.can <= 0) {
    return 'OYUNCU'
  } else if (oyuncu.can <= 0) {
    return 'CANAVAR'
  } else {
    return null
  }
})

var oyuncu = reactive({
  can: 100,
  saldırıGücü: 20,
  savunmaGücü: 100,
  özelAtakHakkı: true,
})

var canavar = reactive({
  can: 100,
  saldırıGücü: 30,
  savunmaGücü: 100,
})

var round = 1;
var roundOlayları = [];

// watch(
//   () => oyuncu.can,
//   (newValue, oldValue) => {
//     //OYUNCUNUN CANI HER DEĞİŞTİĞİNDE BURASI TRİGGERLANIYOR
//
//   },
// )

function Atak(katsayı = 1, b, j, k, l, i, ş) {
  // 0'la 10 arasında rastgele sayı üret
  // Oyuncu Saldırı
  var rastgeleSayi = RastgeleSayıUret(0, oyuncu.saldırıGücü) * katsayı
  canavar.can -= rastgeleSayi
  if (canavar.can < 0) canavar.can = 0

  LogOluştur("Oyuncu","Canavara",rastgeleSayi)

  // Canavar Saldırı
  CanavarSaldır()
  round++;
}

function CanavarSaldır() {
  var rastgeleSayi = RastgeleSayıUret(0, oyuncu.saldırıGücü)
  oyuncu.can -= rastgeleSayi

  if (oyuncu.can < 0) {
    oyuncu.can = 0
  }

  LogOluştur('Canavar', 'Oyuncuya', rastgeleSayi)
}

function OzelAtak() {
  if (!oyuncu.özelAtakHakkı) return
  oyuncu.özelAtakHakkı = false
  Atak(3)
}

function CanBas() {
  // Can Basma Kısmı

    let basılacakCan = RastgeleSayıUret(0, oyuncu.saldırıGücü)
    oyuncu.can += basılacakCan

    if (oyuncu.can > 100) {
      oyuncu.can = 100
    }


  LogOluştur("Oyuncu","kendine",basılacakCan,'canbasma')
  //console.log('CanBasıldı: ' + oyuncu.can)
  CanavarSaldır()
  //console.log('Canavar Saldırdı: ' + oyuncu.can)

  round++
}

function RastgeleSayıUret(min, max) {
  return Math.floor(Math.random() * (max - min + 0)) + min
}

function Restart() {
  oyuncu.can = 100
  oyuncu.can = 100
  oyuncu.özelAtakHakkı = true
}

function LogOluştur(kim, kime, kaçVurdu,aksiyon='vuruş') {
  // Her aksiyonda yeni bir olay oluştur

  // OBJELİ PARAMETRE YAPILACAK !!!!


  // OLAYLAR = ARRAY OF ROUNDS

  var tempOlay = {
    kim: kim,
    kime: kime,
    kaçVurdu: kaçVurdu,
    aksiyon: aksiyon,
    round:round
  }


  roundOlayları.push(tempOlay)


  olaylar.push(roundOlayları)

  console.log(olaylar)
}

function PixelHesapla(maxPx, canDegeri) {
  return (canDegeri * maxPx) / 100 //100 değeri maxCan olarak kabul edildi;
}
</script>

<style scoped>
.text-center {
  text-align: center;
}

.canBarı {
  height: 20px;
  text-align: center;
  color: white;
}

.siyahYazı {
  color: black;
}

.barRenkFULL {
  background-color: limegreen;
}

.barRenkDAMAGED {
  background-color: #f6f279;
}

.barRenkMEDIUM {
  background-color: orange;
}

.barRenkLOW {
  background-color: red;
}
</style>
