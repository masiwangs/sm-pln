<template>
  <div>
    <stats-cards/>
    <v-row>
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Persentase Progress</v-card-title>
          <v-card-text>
            <progress-chart/>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="8">
        <v-card style="height: 386px;">
          <v-card-title>Update Terbaru</v-card-title>
          <v-card-text>
            <div style="height: 300px; overflow-y: scroll">
              <last-update/>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card>
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