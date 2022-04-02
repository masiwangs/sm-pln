<template>
  <v-card elevation="0" class="rounded-lg">
    <v-card-title class="d-flex justify-space-between">
      <div>Basket {{ basket }}</div>
      <v-btn elevation="0" class="rounded-lg" v-if="tahap != 'kontrak'" v-on:click="newProject(basket)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
    </v-card-title>
    <v-divider class="mx-4"></v-divider>
    <v-card-text>
        <prk-table v-if="tahap == 'prk'" :basket="basket" :data_table="data_table" @detail_prk="detailPrk"/>
        <skki-table v-else-if="tahap == 'skki'" :basket="basket"/>
        <pengadaan-table v-else-if="tahap == 'pengadaan'" :basket="basket"/>
        <kontrak-table v-else-if="tahap == 'kontrak'" :basket="basket"/>
        <pelaksanaan-table v-else-if="tahap == 'pelaksanaan'" :basket="basket"/>
    </v-card-text>
    <v-divider  v-if="tahap == 'skki'" class="mx-4"></v-divider>
    <v-card-text v-if="tahap == 'skki'">
        <v-icon>mdi-sigma</v-icon> SKKI: Rp 2.000.000.000,-
    </v-card-text>
  </v-card>
</template>

<script>
import PrkTable from '../table/PrkTable.vue';
import SkkiTable from '../table/SkkiTable.vue';
import PengadaanTable from '../table/PengadaanTable.vue';
import KontrakTable from '../table/KontrakTable.vue';
import PelaksanaanTable from '../table/PelaksanaanTable.vue';
export default {
    components: { PrkTable, SkkiTable, PengadaanTable, KontrakTable, PelaksanaanTable },
    props: ['basket', 'tahap', 'data_table'],
    mounted() {
    },

    methods: {
        detailPrk(data) {
            console.log(data)
        },
        showMaterial(id) {
            let material = this.tbl_items.find(item => item.id == id).materials
            this.$emit('show_material', material)
        },
        showJasa(id) {
            let jasa = this.tbl_items.find(item => item.id == id).jasas
            this.$emit('show_jasa', jasa)
        },
        newProject(no_basket) {
            this.$emit('new_project', no_basket)
        }
    }
}
</script>

<style>

</style>