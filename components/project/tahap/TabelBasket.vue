<template>
  <v-card>
    <v-card-title class="d-flex justify-space-between">
      <div>Basket {{ no_basket }}</div>
      <v-btn v-on:click="buatProjectBaru(1)">Baru</v-btn>
    </v-card-title>
    <v-data-table 
      :headers="tbl_header" 
      :items="tbl_items" 
      :items-per-page="5" 
      item-key="id"
      class="elevation-1 mb-4"
    >
      <template v-slot:item.sum_jasas="{ item }">
        <a href="javascript:void(0)" @click="showJasa(item.id)">
          Rp{{ new Intl.NumberFormat('id-ID').format(item.sum_jasas) }}
        </a>
      </template>
      <template v-slot:item.sum_materials="{ item }">
        <a href="javascript:void(0)" @click="showMaterial(item.id)">
          Rp{{ new Intl.NumberFormat('id-ID').format(item.sum_materials) }}
        </a>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  props: ['no_basket'],
  data() {
    return {
      tbl_header: [{
          text: 'Nama Project',
          value: 'nama_project'
        },
        {
          text: 'PRK',
          value: 'no_prk'
        },
        {
          text: 'RAB Jasa (Rp)',
          value: 'sum_jasas'
        },
        {
          text: 'RAB Material (Rp)',
          value: 'sum_materials'
        }
      ],
      tbl_items: []
    }
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData() {
      this.$axios.get('/prks?basket='+this.no_basket)
        .then(res => {
          this.tbl_items = res.data.data
          console.log(this.tbl_items)
        })
    },
    showMaterial(id) {
      let material = this.tbl_items.find(item => item.id == id).materials
      this.$emit('show_material', material)
    },
    showJasa(id) {
      let jasa = this.tbl_items.find(item => item.id == id).jasas
      this.$emit('show_jasa', jasa)
    }
  }
}
</script>

<style>

</style>