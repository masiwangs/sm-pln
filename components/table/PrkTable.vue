<template>
    <v-data-table 
        :headers="tbl_header" 
        :items="data_table" 
        :items-per-page="5"
        class="elevation-0 mb-4"
    >
        <template v-slot:item.nama_project="{ item }">
            <a href="javascript:void(0)" style="text-decoration:none" @click="$emit('show_detail', item)">{{ item.nama_project }}</a>
        </template>
    </v-data-table>
</template>

<script>
export default {
    props: ['data_table'],
    data() {
        return {
        tbl_header: [
            {
                text: 'Nama Project',
                value: 'nama_project'
            },
            {
                text: 'PRK',
                value: 'nomor_prk'
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
}
</script>

<style>

</style>