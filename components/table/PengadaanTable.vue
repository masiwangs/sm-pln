<template>
<div>
    <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        placeholder="Cari"
        filled
        rounded
        dense
        :hide-details="true"
        style="max-width: 20rem"
        class="mb-3"
    ></v-text-field>
    <v-data-table 
        :headers="tbl_header" 
        :items="data" 
        :items-per-page="5" 
        item-key="id"
        :search="search"
        class="elevation-0 mb-4"
    >
        <template v-slot:item.nodin="{ item }">
            <nuxt-link 
                :to="'/projects/pengadaan/'+item.id"
                @click="$emit('show_detail', item)"
            >{{ item.nodin ? item.nodin : 'Untitled' }}</nuxt-link>
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
    props: ['data'],
    data() {
        return {
            search: '',
            tbl_header: [
                {
                    text: 'Nodin',
                    value: 'nodin'
                },
                {
                    text: 'Tgl. Nodin',
                    value: 'tanggal_nodin'
                },
                {
                    text: 'Nama Project',
                    value: 'nama_project'
                },
                {
                    text: 'RAB Jasa (Rp)',
                    value: 'jasas_sum'
                },
                {
                    text: 'RAB Material (Rp)',
                    value: 'materials_sum'
                },
                {
                    text: 'Status',
                    value: 'status'
                },
            ],
        }
    },
    
    watch: {
        data(val) {
            console.log(val)
        }
    }
}
</script>

<style>

</style>