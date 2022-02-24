<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">Detail Project</h4>
        <h2>Project Pembangunan Jalan</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card elevation="2">
        <v-card-title>
          Informasi Project
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12" md="6">
              <v-text-field outlined dense label="No. PRK" value="123X"></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field outlined dense label="No. SKKI" value="123X"></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field outlined dense label="No. WBS" value="123X"></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field outlined dense label="No. PRK-SKKI" value="123X"></v-text-field>
            </v-col>
          </v-row>
          <div class="d-flex justify-end">
            <v-btn elevation="2" color="primary">
              <v-icon class="me-2">mdi-content-save</v-icon> Simpan
            </v-btn>
          </div>
        </v-card-text>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title>Jasa</v-card-title>
        <v-card-text>
          <v-data-table :headers="tableJasaHeader" :items="tableJasaItems" :items-per-page="5" class="elevation-1 mb-4"></v-data-table>
        </v-card-text>
      </v-card>
    </v-col>

    <v-col cols="12">
      <v-card>
        <v-card-title>Material</v-card-title>
        <v-card-text>
          <v-data-table :headers="tableJasaHeader" :items="tableJasaItems" :items-per-page="5" class="elevation-1 mb-4"></v-data-table>
        </v-card-text>
      </v-card>
    </v-col>

    <v-col cols="12">
      <detail-pengadaan/>
    </v-col>

    <v-col cols="12">
      <detail-kontrak/>
    </v-col>
  </v-row>
</template>

<script>
import DetailPrk from '~/components/project/DetailPrk.vue'
import DetailSkki from '~/components/project/DetailSkki.vue'
import DetailPengadaan from '~/components/project/DetailPengadaan.vue'
import DetailKontrak from '~/components/project/DetailKontrak.vue'
import DetailPelaksanaan from '~/components/project/DetailPelaksanaan.vue'
import DetailPembayaran from '~/components/project/DetailPembayaran.vue'
  export default {
    components: {
      DetailPrk,
      DetailSkki,
      DetailPengadaan,
      DetailKontrak,
      DetailPelaksanaan,
      DetailPembayaran,
    },
    data() {
      return {
        breadcrumbItems: [{
            text: 'Dashboard',
            active: true,
            href: '/'
          },
          {
            text: 'Project',
            active: true,
            href: '/project'
          },
          {
            text: 'Detail',
            active: false,
          },
        ],
        tableJasaHeader: [{
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          {
            text: 'Calories',
            value: 'calories'
          },
          {
            text: 'Fat (g)',
            value: 'fat'
          },
          {
            text: 'Carbs (g)',
            value: 'carbs'
          },
          {
            text: 'Protein (g)',
            value: 'protein'
          },
          {
            text: 'Iron (%)',
            value: 'iron'
          },
        ],
        tableJasaItems: [{
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%',
          },
        ],
      }
    },
    mounted() {
      let query = this.$route.query
      console.log(1)
    }
  }
</script>

<style>

</style>
