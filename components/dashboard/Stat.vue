<template>
  <v-card elevation="0" style="position: relative" class="rounded-lg">
        <v-card-title>
            <span :class="color+'--text'">
                {{ label }}
            </span>
        </v-card-title>
        <v-card-text>
            <div>
                <v-tooltip bottom v-if="label == 'Pembayaran'">
                    <template v-slot:activator="{ on, attrs }">
                        <h4 
                            v-bind="attrs"
                            v-on="on"
                            :class="'mb-3'"
                        >
                            {{ humanizeNumber(dataJasa) }}
                        </h4>
                    </template>
                    {{ formatIdr(dataJasa) }}
                </v-tooltip>
                <v-tooltip bottom v-else>
                    <template v-slot:activator="{ on, attrs }">
                        <h4 
                            v-bind="attrs"
                            v-on="on"
                            class="mb-3"
                        >
                            {{ humanizeNumber(dataJasa + dataMaterial) }}
                        </h4>
                    </template>
                    <span>{{ formatIdr(dataJasa + dataMaterial) }}</span>
                </v-tooltip>
                <div v-show="detail && label !== 'Pembayaran'" color="white">
                    <span :class="color+'--text'">Jasa</span>
                    <v-tooltip bottom>
                        <template v-slot:activator="{ on, attrs }">
                            <h4 
                                v-bind="attrs"
                                v-on="on"
                                class="mb-3 font-weight-light"
                            >
                                {{ humanizeNumber(dataJasa) }}
                            </h4>
                        </template>
                        <span>{{ formatIdr(dataJasa) }}</span>
                    </v-tooltip>
                    <span :class="color+'--text'">Material</span>
                    <v-tooltip bottom>
                        <template v-slot:activator="{ on, attrs }">
                            <h4 
                                v-bind="attrs"
                                v-on="on"
                                class="mb-3 font-weight-light"
                            >
                                {{ humanizeNumber(dataMaterial) }}
                            </h4>
                        </template>
                        <span class="font-weight-light">{{ formatIdr(dataMaterial) }}</span>
                    </v-tooltip>
                </div>
                <div class="text-right">
                    <!-- <v-chip v-if="item.percentage" small :color="item.color+' lighten-1'">
                        {{ formatPercent(item.percentage) }}
                    </v-chip>
                    <div v-else>&nbsp;</div> -->
                </div>
            </div>
            <v-btn v-on:click="detail = !detail" block elevation="0" x-small class="mt-2 grey--text" color="white">
                <div v-if="label !== 'Pembayaran'">
                    <div v-if="!detail">
                        <v-icon style="font-size: .9rem">mdi-eye</v-icon><span style="font-size: .7rem">&nbsp;Detail</span>
                    </div>
                    <div v-else>
                        <v-icon style="font-size: .9rem">mdi-eye-off-outline</v-icon><span style="font-size: .7rem">&nbsp;Sembunyikan</span>
                    </div>
                </div>
                <div v-else>&nbsp;</div>
            </v-btn>
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    props: ['label', 'color', 'detail', 'dataJasa', 'dataMaterial'],
    methods: {
    toogleDetail(index) {
      return this.detail[index] = !this.detail[index]
    },
    safePercent(number, attacker) {
            if(attacker == 0) {
                return number;
            }

            return number/attacker
        },
        formatIdr(number) {
            return new Intl.NumberFormat('id-ID', {style: 'currency', currency: 'IDR'}).format(number)
        },
        formatPercent(number) {
            return new Intl.NumberFormat('id-ID', {maximumSignificantDigits: 2}).format(number)+'%'
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
        }
    },
}
</script>

<style>

</style>