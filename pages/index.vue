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
                        dense
                        :hide-details="true"
                        v-model="selectedBasket"
                        v-on:change="handleChangeBasket()"
                    ></v-select>
                </v-form>
            </v-card-text>
        </v-card>

        <stats-cards :data="statsData"/>

        <percentage-card :data="statsData"/>
        
        <v-row>
            <v-col cols="12">
                <v-card class="rounded-lg" elevation="0">
                    <v-card-title>Biaya Project</v-card-title>
                    <v-card-text>
                        <project-cost :chartData="chartData" :chartOptions="chartOptions"/>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
    </div>
</template>

<script>
import LastUpdate from '~/components/dashboard/LastUpdate.vue'
import ProjectCost from '~/components/dashboard/ProjectCost.vue'
import StatsCards from '~/components/dashboard/StatsCards.vue'
import PercentageCard from '~/components/dashboard/PercentageCard.vue'
export default {
    components: { ProjectCost, LastUpdate, StatsCards, PercentageCard },
    data() {
        return {
            statsDetail: false,
            selectedBasket: 'Semua',
            statsData: [],
            percentageData: [],
            chartData: {},
            chartOptions: {}
        }
    },
    async fetch() {
        let res = await this.handleChangeBasket()
    },
    methods: {
        async handleChangeBasket() {
            let basket = '';
            if(this.selectedBasket != 'Semua') {
                basket = this.selectedBasket.slice(-1)
            }

            let res = await this.$axios.get(`stats/biaya?basket=${basket}`)
            this.statsData = res.data.data;

            let jasa = [], material = [], sum = [];
            let temp = 0;
            res.data.data.forEach((v, i) => {
                if(i%2 == 0) {
                    jasa.push(v)
                    if(res.data.data[i+1]) {
                        temp = v;
                    } else {
                        sum.push(v)
                    }
                } else {
                    material.push(v)
                    temp += v;
                    sum.push(temp)
                    temp = 0
                }
            })

            this.chartData = {
                labels: ['PRK', 'SKKI', 'Pengadaan', 'Kontrak', 'Pelaksanaan', 'Pembayaran'],
                datasets: [
                    {
                        label: 'Material',
                        backgroundColor: '#008FFB',
                        data: material
                    }, {
                        label: 'Jasa',
                        backgroundColor: '#00E396',
                        data: jasa
                    }, {
                        label: 'Total',
                        backgroundColor: '#FEB019',
                        data: sum
                    }
                ]
            };

            this.chartOptions = {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        ticks: {
                            callback: function(value, index, ticks) {
                                let thousands = 0;
                                let copy_number = value;
                                while(copy_number > 1000) {
                                    copy_number = copy_number/1000;
                                    thousands++;
                                }

                                if(thousands == 0) {
                                    return 'Rp '+new Intl.NumberFormat('id-ID').format(value);
                                }

                                let text = '';
                                if(thousands == 1) {
                                    text = 'rb';
                                } else if(thousands == 2) {
                                    text = 'jt';
                                } else if(thousands == 3) {
                                    text = 'M';
                                } else if(thousands == 4) {
                                    text = 'T';
                                }

                                return 'Rp '+new Intl.NumberFormat('id-ID').format(copy_number)+text;
                            }
                        }
                    }
                }
            }
        }
    }
}
</script>

<style scoped>

</style>