<template>
    <v-data-table 
        :headers="tbl_header" 
        :items="tbl_items" 
        :items-per-page="5" 
        item-key="id"
        class="elevation-0 mb-4"
    >
    </v-data-table>
</template>

<script>
export default {
    data() {
        return {
        tbl_header: [
            {
                text: 'Nodin',
                value: 'nodin'
            },
            {
                text: 'Tgl. Nodin',
                value: 'tgl_nodin'
            },
            {
                text: 'Nama Project',
                value: 'nama'
            },
            {
                text: 'RAB Jasa',
                value: 'rab_jasa'
            },
            {
                text: 'RAB Material',
                value: 'rab_material'
            },
            {
                text: 'Status',
                value: 'status'
            },
        ],
        tbl_items: []
        }
  },
}
</script>

<style>

</style>