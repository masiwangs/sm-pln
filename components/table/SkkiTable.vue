<template>
    <v-data-table 
        :headers="tbl_header" 
        :items="data" 
        :items-per-page="5" 
        item-key="id"
        class="elevation-0 mb-4"
    >
        <template v-slot:item.nomor_skki="{ item }">
            <a 
                href="javascript:void(0)"
                style="text-decoration: none"
                @click="$emit('show_detail', item)"
            >{{ item.nomor_skki ? item.nomor_skki : 'Untitled' }}</a>
        </template>
    </v-data-table>
</template>

<script>
export default {
    props: ['data'],
    data() {
        return {
            tbl_header: [
                {
                    text: 'Nomor SKKI',
                    value: 'nomor_skki'
                },
                {
                    text: 'Nomor PRK-SKKI',
                    value: 'nomor_prk_skki'
                },
                {
                    text: 'WBS Jasa',
                    value: 'nomor_wbs_jasa'
                },
                {
                    text: 'WBS Material',
                    value: 'nomor_wbs_material'
                },
                {
                    text: 'RAB Jasa (Rp)',
                    value: 'sum_jasas'
                },
            ],
        }
    },
}
</script>

<style>

</style>