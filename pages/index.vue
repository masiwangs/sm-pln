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

        <stats-cards/>

        <v-row>
            <v-col cols="12" md="4">
                <v-card
                    elevation="0"
                    class="rounded-lg"
                >
                    <v-card-title>Persentase Kontrak/SKKI</v-card-title>
                    <v-card-text class="d-flex flex-row align-center">
                        <div class="me-auto">
                            <p style="font-size: 3.5rem;line-height: 3.5rem">60%</p>
                            <p class="mb-0">Rp19.000.000/Rp28.000.000</p>
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
                            <p style="font-size: 3.5rem;line-height: 3.5rem">60%</p>
                            <p class="mb-0">Rp19.000.000/Rp28.000.000</p>
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
                            <p style="font-size: 3.5rem;line-height: 3.5rem">60%</p>
                            <p class="mb-0">Rp19.000.000/Rp28.000.000</p>
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
                        <project-cost/>
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
import StatsCards from '~/components/dashboard/StatsCards.vue'
export default {
  components: { ProgressChart, StatsCards, ProjectCost, LastUpdate },
  data() {
    return {
      
      
    }
  }
}
</script>

<style scoped>

</style>