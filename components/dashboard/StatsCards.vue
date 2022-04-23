<template>
  <v-row>
    <v-col v-for="(item, index) in statsCards" :key="index" cols="12" md="6" lg="2" style="color: white">
      <v-card elevation="0" style="position: relative" v-on:click="statsDetail = !statsDetail" class="rounded-lg">
        <v-card-title :class="item.color+'--text'">
          {{ item.title }}
        </v-card-title>
        <v-card-text>
          <div>
            <h4 :class="'mb-3'">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.jasa + item.material) }}
            </h4>
            <div class="text-right">
              <v-chip v-if="item.percentage" small :color="item.color+' lighten-1'">
                {{ item.percentage }}%
              </v-chip>
              <div v-else>&nbsp;</div>
            </div>
          </div>
        </v-card-text>
        <v-card-text v-show="statsDetail" color="white">
          <span :class="item.color+'--text'">Jasa</span>
          <h4>
              Rp{{ new Intl.NumberFormat('id-ID').format(item.jasa) }}
          </h4>
          <span :class="item.color+'--text'">Material</span>
          <h4>
              Rp{{ new Intl.NumberFormat('id-ID').format(item.material) }}
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
                    jasa: 0,
                    material: 0,
                    percentage: null,
                    color: 'primary',
                    icon: 'puzzle'
                },
                {
                    title: 'SKKI',
                    value: 19000000000,
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'red',
                    icon: 'note-text'
                },
                {
                    title: 'Pengadaan',
                    value: 19000000000,
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'amber',
                    icon: 'cart'
                },
                {
                    title: 'Kontrak',
                    value: 19000000000,
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'success',
                    icon: 'file-sign'
                },
                {
                    title: 'Pelaksanaan',
                    value: 19000000000,
                    jasa: 0,
                    material: 0,
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
                prk_jasa: 0,
                prk_material: 0,
                skki_jasa: 0,
                skki_material: 0,
                pengadaan_jasa: 0,
                pengadaan_material: 0,
                kontrak_jasa: 0,
                kontrak_material: 0,
                pelaksanaan_jasa: 0,
                pelaksanaan_material: 0,
                pembayaran: 0,
            }
        },
        async fetch() {
            let res = await this.$axios.get('stats/biaya')
            this.statsCards[0].jasa = res.data.data[0];
            this.statsCards[0].material = res.data.data[1];
            this.statsCards[1].jasa = res.data.data[2];
            this.statsCards[1].material = res.data.data[3];
            this.statsCards[2].jasa = res.data.data[4];
            this.statsCards[2].material = res.data.data[5];
            this.statsCards[3].jasa = res.data.data[6];
            this.statsCards[3].material = res.data.data[7];
            this.statsCards[4].jasa = res.data.data[8];
            this.statsCards[4].material = res.data.data[9];
        },
        mounted() {
            // this.load();
        },
        methods: {
            load() {
                this.$axios.get('stats/biaya')
                    .then(res => {
                        this.statsCards[0].jasa = res.data.data[0];
                        this.statsCards[0].material = res.data.data[1];
                        this.statsCards[1].jasa = res.data.data[2];
                        this.statsCards[1].material = res.data.data[3];
                        this.statsCards[2].jasa = res.data.data[4];
                        this.statsCards[2].material = res.data.data[5];
                        this.statsCards[3].jasa = res.data.data[6];
                        this.statsCards[3].material = res.data.data[7];
                        this.statsCards[4].jasa = res.data.data[8];
                        this.statsCards[4].material = res.data.data[9];
                    })
            }
        }
    }
</script>

<style>

</style>
