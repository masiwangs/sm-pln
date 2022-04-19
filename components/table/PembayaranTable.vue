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
            <template v-slot:item.spk="{ item }">
                <nuxt-link
                    :to="'/projects/pembayaran/'+item.id"
                >
                    {{ item.spk }}
                </nuxt-link>
            </template>
            <template v-slot:item.tagihan="{ item }">
                Rp{{ new Intl.NumberFormat('id-ID').format(item.tagihan) }},-
            </template>
            <template v-slot:item.pembayarans="{ item }">
                <ol v-if="item.pembayarans.length > 0">
                    <li v-for="pembayaran in item.pembayarans">Rp{{ new Intl.NumberFormat('id-ID').format(pembayaran.nominal) }}</li>
                </ol>
                <div v-else>
                    Belum ada pembayaran
                </div>
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
                    text: 'SPK',
                    value: 'spk'
                }, 
                {
                    text: 'Tagihan',
                    value: 'tagihan'
                },
                {
                    text: 'Pembayaran',
                    value: 'pembayarans'
                },
                {
                    text: 'Status',
                    value: 'status'
                },
            ]
        }
    },
}
</script>

<style>

</style>