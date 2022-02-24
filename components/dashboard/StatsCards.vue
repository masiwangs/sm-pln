<template>
  <v-row>
    <v-col v-for="(item, index) in statsCards" :key="index" cols="12" md="6" lg="2" style="color: white">
      <v-card elevation="1" :color="item.color" style="position: relative" v-on:click="statsDetail = !statsDetail">
        <div style="position: absolute; top: 10px; right: 10px">
          <v-icon large :color="item.color+' darken-3'">mdi-{{ item.icon }}</v-icon>
        </div>
        <v-card-title class="white--text">
          {{ item.title }}
        </v-card-title>
        <v-card-text>
          <div>
            <h4 class="white--text mb-3">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.value) }}
            </h4>
            <div class="text-right">
              <v-chip v-if="item.percentage" small :color="item.color+' lighten-1'">
                {{ item.percentage }}%
              </v-chip>
              <div v-else>&nbsp;</div>
            </div>
          </div>
        </v-card-text>
      </v-card>
      <v-card v-show="statsDetail" color="white">
        <v-card-text>
          <span>Jasa</span>
          <h4 :class="item.color+'--text'">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.value) }}
          </h4>
          <span>Material</span>
          <h4 :class="item.color+'--text'">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.value) }}
          </h4>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
  export default {
    data() {
      return {
        statsCards: [{
            title: 'PRK',
            value: 19000000000,
            percentage: null,
            color: 'primary',
            icon: 'puzzle'
          },
          {
            title: 'SKKI',
            value: 19000000000,
            percentage: 100,
            color: 'red',
            icon: 'note-text'
          },
          {
            title: 'Pengadaan',
            value: 19000000000,
            percentage: 100,
            color: 'amber',
            icon: 'cart'
          },
          {
            title: 'Kontrak',
            value: 19000000000,
            percentage: 100,
            color: 'success',
            icon: 'file-sign'
          },
          {
            title: 'Pelaksanaan',
            value: 19000000000,
            percentage: 100,
            color: 'purple',
            icon: 'crane'
          },
          {
            title: 'Pembayaran',
            value: 19000000000,
            percentage: 100,
            color: 'secondary',
            icon: 'currency-usd'
          }
        ],
        statsDetail: false,
      }
    }
  }
</script>

<style>

</style>
