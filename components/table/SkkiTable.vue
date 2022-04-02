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
                text: 'PRK-SKKI',
                value: 'prk_skki'
            },
            {
                text: 'WBS',
                value: 'no_wbs'
            },
            {
                text: 'RAB Jasa (Rp)',
                value: 'sum_jasas'
            },
        ],
        tbl_items: []
        }
  },
}
</script>

<style>

</style>