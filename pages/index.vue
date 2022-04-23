<template>
    <div>
        <v-card
            elevation="0"
            class="mb-6 rounded-lg"
        >
            <v-card-text class="d-flex flex-row align-center">
                <p class="me-auto">Filter informasi dashboard:</p>
                <v-form>
                    <v-select
                        :items="['Semua', 'Basket 1', 'Basket 2', 'Basket 3']"
                        filled
                        rounded
                        value="Semua"
                        dense
                        :hide-details="true"
                    ></v-select>
                </v-form>
            </v-card-text>
        </v-card>

        <v-row>
            <v-col v-for="(item, index) in statsCards" :key="index" cols="12" md="6" lg="2" style="color: white">
                <v-card elevation="0" style="position: relative" v-on:click="statsDetail = !statsDetail" class="rounded-lg">
                    <v-card-title :class="item.color+'--text'">{{ item.title }}</v-card-title>
                    <v-card-text>
                        <div>
                            <h4 v-if="item.title == 'Pembayaran'" :class="'mb-3'">
                                {{ formatIdr(item.value) }}
                            </h4>
                            <h4 v-else :class="'mb-3'">
                                {{ formatIdr(item.jasa + item.material) }}
                            </h4>
                            <div class="text-right">
                                <v-chip v-if="item.percentage" small :color="item.color+' lighten-1'">
                                    {{ formatPercent(item.percentage) }}
                                </v-chip>
                                <div v-else>&nbsp;</div>
                            </div>
                        </div>
                    </v-card-text>
                    <v-card-text v-show="statsDetail && item.title !== 'Pembayaran'" color="white">
                        <span :class="item.color+'--text'">Jasa</span>
                        <h4>
                            {{ formatIdr(item.jasa) }}
                        </h4>
                        <span :class="item.color+'--text'">Material</span>
                        <h4>
                            {{ formatIdr(item.material) }}
                        </h4>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>

        <v-row>
            <v-col cols="12" md="4">
                <v-card
                    elevation="0"
                    class="rounded-lg"
                >
                    <v-card-title>Persentase Kontrak/SKKI</v-card-title>
                    <v-card-text class="d-flex flex-row align-center">
                        <div class="me-auto">
                            <p style="font-size: 3.5rem;line-height: 3.5rem">{{ 
                                formatPercent((statsCards.kontrak.jasa + statsCards.kontrak.material)/(statsCards.skki.jasa +  statsCards.skki.material)*100)
                            }}</p>
                            <p class="mb-0">{{ formatIdr(statsCards.kontrak.jasa + statsCards.kontrak.material) }}/{{ formatIdr(statsCards.skki.jasa + statsCards.skki.material) }}</p>
                        </div>
                        <progress-chart/>
                    </v-card-text>
                </v-card>
            </v-col>
            <v-col cols="12" md="4">
                <v-card
                elevation="0"
                class="rounded-lg"
                >
                    <v-card-title>Persentase Bayar/SKKI</v-card-title>
                    <v-card-text class="d-flex flex-row align-center">
                        <div class="me-auto">
                            <p style="font-size: 3.5rem;line-height: 3.5rem">{{ 
                                formatPercent((statsCards.pembayaran.value)/(statsCards.skki.jasa + statsCards.skki.material)*100) 
                            }}</p>
                            <p class="mb-0">{{ formatIdr(statsCards.pembayaran.value) }}/{{ formatIdr(statsCards.skki.jasa + statsCards.skki.material) }}</p>
                        </div>
                        <progress-chart/>
                    </v-card-text>
                </v-card>
            </v-col>
            <v-col cols="12" md="4">
                <v-card
                elevation="0"
                class="rounded-lg"
                >
                    <v-card-title>Persentase Bayar/Kontrak</v-card-title>
                    <v-card-text class="d-flex flex-row align-center">
                        <div class="me-auto">
                            <p style="font-size: 3.5rem;line-height: 3.5rem">{{ 
                                formatPercent((statsCards.pembayaran.value)/(statsCards.kontrak.jasa + statsCards.kontrak.material)*100)
                            }}</p>
                            <p class="mb-0">{{ formatIdr(statsCards.pembayaran.value) }}/{{ formatIdr(statsCards.kontrak.jasa + statsCards.kontrak.material) }}</p>
                        </div>
                        <progress-chart/>
                    </v-card-text>
                </v-card>
            </v-col>
            
            </v-row>
            <v-row>
            <v-col cols="12">
                <v-card class="rounded-lg" elevation="0">
                    <v-card-title>Biaya Project</v-card-title>
                    <v-card-text>
                        <apexchart type="bar" width="100%" height="350" :options="chartOptions" :series="chartSeries"></apexchart>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
    </div>
</template>

<script>
import LastUpdate from '~/components/dashboard/LastUpdate.vue'
import ProgressChart from '~/components/dashboard/ProgressChart.vue'
import ProjectCost from '~/components/dashboard/ProjectCost.vue'
export default {
    components: { ProgressChart, ProjectCost, LastUpdate },
    data() {
        return {
            statsDetail: false,
            statsCards: {
                prk: {
                    title: 'PRK',
                    jasa: 0,
                    material: 0,
                    percentage: null,
                    color: 'primary',
                    icon: 'puzzle'
                },
                skki: {
                    title: 'SKKI',
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'red',
                    icon: 'note-text'
                },
                pengadaan: {
                    title: 'Pengadaan',
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'amber',
                    icon: 'cart'
                },
                kontrak: {
                    title: 'Kontrak',
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'success',
                    icon: 'file-sign'
                },
                pelaksanaan: {
                    title: 'Pelaksanaan',
                    jasa: 0,
                    material: 0,
                    percentage: 100,
                    color: 'purple',
                    icon: 'crane'
                },
                pembayaran: {
                    title: 'Pembayaran',
                    value: 0,
                    percentage: 100,
                    color: 'secondary',
                    icon: 'currency-usd'
                }
            },
            chartSeries: [],
            chartOptions: {
                chart: {
                    type: 'bar',
                    width: '100%'
                },
                plotOptions: {
                    bar: {
                    horizontal: false,
                    columnWidth: '55%',
                    endingShape: 'rounded'
                    },
                },
                dataLabels: {
                    enabled: false
                },
                stroke: {
                    show: true,
                    width: 2,
                    colors: ['transparent']
                },
                xaxis: {
                    categories: ['PRK', 'SKKI', 'Pengadaan', 'Kontrak', 'Pelaksanaan', 'Pembayaran'],
                },
                yaxis: {
                    title: {
                    text: 'Rp'
                    }
                },
                fill: {
                    opacity: 1
                },
                tooltip: {
                    y: {
                        formatter: function (val) {
                            return new Intl.NumberFormat('id-ID', {style: 'currency', currency: 'IDR'}).format(val)
                        }
                    }
                }
            },
        }
    },
    async fetch() {
        let res = await this.$axios.get('stats/biaya')
        console.log(res)
        this.statsCards.prk.jasa = res.data.data[0];
        this.statsCards.prk.material = res.data.data[1];
        this.statsCards.skki.jasa = res.data.data[2];
        this.statsCards.skki.material = res.data.data[3];
        this.statsCards.pengadaan.jasa = res.data.data[4];
        this.statsCards.pengadaan.material = res.data.data[5];
        this.statsCards.kontrak.jasa = res.data.data[6];
        this.statsCards.kontrak.material = res.data.data[7];
        this.statsCards.pelaksanaan.jasa = res.data.data[8];
        this.statsCards.pelaksanaan.material = res.data.data[9];
        this.statsCards.pembayaran.value = res.data.data[10];

        let material_series = [
            this.statsCards.prk.material,
            this.statsCards.skki.material,
            this.statsCards.pengadaan.material,
            this.statsCards.kontrak.material,
            this.statsCards.pelaksanaan.material,
            0
        ]
        let jasa_series = [
            this.statsCards.prk.jasa,
            this.statsCards.skki.jasa,
            this.statsCards.pengadaan.jasa,
            this.statsCards.kontrak.jasa,
            this.statsCards.pelaksanaan.jasa,
            0
        ]
        let total_series = [
            this.statsCards.prk.jasa + this.statsCards.prk.material,
            this.statsCards.skki.jasa + this.statsCards.skki.material,
            this.statsCards.pengadaan.jasa + this.statsCards.pengadaan.material,
            this.statsCards.kontrak.jasa + this.statsCards.kontrak.material,
            this.statsCards.pelaksanaan.jasa + this.statsCards.pelaksanaan.material,
            this.statsCards.pembayaran.value
        ]

        this.chartSeries = [
            {
                name: 'Material',
                data: material_series
            }, {
                name: 'Jasa',
                data: jasa_series
            }, {
                name: 'Total',
                data: total_series
            }
        ]
    },
    methods: {
        formatIdr(number) {
            return new Intl.NumberFormat('id-ID', {style: 'currency', currency: 'IDR'}).format(number)
        },
        formatPercent(number) {
            return new Intl.NumberFormat('id-ID', {maximumSignificantDigits: 2}).format(number)+'%'
        }
    }
}
</script>

<style scoped>

</style>