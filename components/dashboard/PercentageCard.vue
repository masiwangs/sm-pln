<template>
  <v-row>
        <v-col cols="12" md="4">
            <v-card
                elevation="0"
                class="rounded-lg"
            >
                <v-card-title>Persentase Kontrak/SKKI</v-card-title>
                <v-card-text class="d-flex flex-row align-center">
                    <div class="me-auto">
                        <p style="font-size: 2.5rem;line-height: 3.5rem">
                            {{ humanizePercent(kontrak, skki) }}
                        </p>
                        <p class="mb-0">{{ humanizeNumber(kontrak) }}/{{ humanizeNumber(skki) }}</p>
                    </div>
                    <progress-chart :chart-data="safeDivider(kontrak, skki)"/>
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
                        <p style="font-size: 2.5rem;line-height: 3.5rem">
                            {{ humanizePercent(pembayaran, skki) }}
                        </p>
                        <p class="mb-0">{{ humanizeNumber(pembayaran) }}/{{ humanizeNumber(skki) }}</p>
                    </div>
                    <progress-chart :chart-data="safeDivider(pembayaran, skki)"/>
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
                        <p style="font-size: 2.5rem;line-height: 3.5rem">{{ 
                            humanizePercent(pembayaran, kontrak)
                        }}</p>
                        <p class="mb-0">{{ humanizeNumber(pembayaran) }}/{{ humanizeNumber(kontrak) }}</p>
                    </div>
                    <progress-chart :chart-data="safeDivider(pembayaran, kontrak)"/>
                </v-card-text>
            </v-card>
        </v-col>
            
    </v-row>
</template>

<script>
import ProgressChart from '~/components/dashboard/ProgressChart.vue'
export default {
    components: {ProgressChart},
    props: ['data'],
    methods: {
        humanizePercent(number, attacker) {
            if(attacker == 0) {
                return new Intl.NumberFormat('id-ID').format(number*100)+'%';
            }

            console.log(number, attacker)
            return new Intl.NumberFormat('id-ID').format(number/attacker*100)+'%';
        },
        humanizeNumber(number) {
            let thousands = 0;
            let copy_number = number;
            while(copy_number > 1000) {
                copy_number = copy_number/1000;
                thousands++;
            }

            if(thousands == 0) {
                return 'Rp '+new Intl.NumberFormat('id-ID').format(number);
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
        },
        safeDivider(number, attacker) {
            if(attacker == 0) {
                return number;
            }

            return number/attacker;
        }
    },
    computed: {
        kontrak() {
            return this.data[6] + this.data[7]
        },
        skki() {
            return this.data[2] + this.data[3]
        },
        pembayaran() {
            return this.data[10]
        },
    },
    
}
</script>

<style>

</style>