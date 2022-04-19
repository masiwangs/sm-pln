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
            class="elevstion-0 mb-4"
        >
            <template v-slot:item.nomor_kontrak="{ item }">
                <nuxt-link
                    :to="'/projects/kontrak/'+item.id"
                >
                    {{ item.nomor_kontrak }}
                </nuxt-link>
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
                    text: 'Nomor Kontrak',
                    value: 'nomor_kontrak'
                }, 
                {
                    text: 'Tanggal Kontrak',
                    value: 'tanggal_kontrak'
                },
                {
                    text: 'Tanggal Awal',
                    value: 'tanggal_awal'
                },
                {
                    text: 'Tanggal Akhir',
                    value: 'tanggal_akhir'
                },
                {
                    text: 'Pelaksana',
                    value: 'pelaksana'
                },
                {
                    text: 'Direksi',
                    value: 'direksi_pelaksana'
                }
            ]
        }
    },
}
</script>

<style>

</style>