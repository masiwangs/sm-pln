<template>
  <v-card class="py-4">
    <v-card-text>
      <div class="d-flex justify-space-between align-center py-2">
        <h3>Informasi Progress</h3>
        <v-btn small elevation="2" color="primary">
          <v-icon class="me-2">mdi-content-save</v-icon> Simpan
        </v-btn>
      </div>
      <v-divider class="mb-6"></v-divider>
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field outlined dense label="No. PRK" value="123X"></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field outlined dense label="No. WBS" value="WBS123"></v-text-field>
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field outlined dense label="No. PRK-SKKI" value="123XSKKI"></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-textarea outlined dense rows="4" label="Kegiatan"
            value="Lorem ipsum dolor, sit amet consectetur adipisicing elit. Accusamus enim soluta esse est iste aliquam tenetur, rerum error, aut blanditiis animi eaque tempore vitae maiores. Accusantium saepe dolor sunt nisi.">
          </v-textarea>
        </v-col>
      </v-row>

      <div class="mt-6 d-flex justify-space-between align-center py-2">
        <h3>RAB Jasa</h3>
        <v-btn small elevation="2" color="primary">
          <v-icon class="me-2">mdi-plus</v-icon> Jasa baru
        </v-btn>
      </div>
      <v-divider class="mb-6"></v-divider>
      <v-data-table :headers="tableJasaHeader" :items="tableJasaItems" :items-per-page="5" class="elevation-1 mb-4">
      </v-data-table>

      <div class="mt-6 d-flex justify-space-between align-center py-2">
        <h3>RAB Material</h3>
        <v-btn small elevation="2" color="primary">
          <v-icon class="me-2">mdi-plus</v-icon> Material baru
        </v-btn>
      </div>
      <v-divider class="mb-6"></v-divider>
      <v-data-table :headers="tableJasaHeader" :items="tableJasaItems" :items-per-page="5" class="elevation-1">
      </v-data-table>
    </v-card-text>
  </v-card>
</template>

<script>
  export default {
    name: 'DetailPrk',
    data() {
      return {
        tableJasaHeader: [
        {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
      ],
      tableJasaItems: [
          {
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
    }
  }
</script>

<style>

</style>
