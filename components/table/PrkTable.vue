<template>
    <div>
        <v-data-table 
            :headers="tbl_header" 
            :items="data_table" 
            :items-per-page="5"
            class="elevation-0 mb-4"
        >
            <template v-slot:item.nama_project="{ item }">
                <nuxt-link 
                    :to="'/projects/prk/'+item.id" 
                    style="text-decoration:none" 
                >{{ item.nama_project ? item.nama_project : 'Untitled' }}</nuxt-link>
            </template>

            <template v-slot:item.jasas_sum="{ item }">
                Rp{{ new Intl.NumberFormat('id-ID').format(item.jasas_sum) }}
            </template>
            <template v-slot:item.materials_sum="{ item }">
                Rp{{ new Intl.NumberFormat('id-ID').format(item.materials_sum) }}
            </template>
        </v-data-table>
    </div>
</template>

<script>
export default {
    props: ['data_table', 'loading'],
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
                    value: 'jasas_sum'
                },
                {
                    text: 'RAB Material (Rp)',
                    value: 'materials_sum'
                }
            ],
            tbl_items: []
        }
    },
}
</script>

<style>

</style>