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
        :search="search"
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
        <template v-slot:item.jasas_sum="{ item }">
            Rp{{ new Intl.NumberFormat('id-ID').format(item.jasas_sum) }}
        </template>
        <template v-slot:item.materials_sum="{ item }">
            Rp{{ new Intl.NumberFormat('id-ID').format(item.materials_sum) }}
        </template>
        <template v-slot:body.append>
            <tr class="grey lighten-3">
                <td colspan="4"><strong>Total</strong></td>
                <td><strong>Rp{{ new Intl.NumberFormat('id-ID').format(rab.jasa) }}</strong></td>
                <td><strong>Rp{{ new Intl.NumberFormat('id-ID').format(rab.material) }}</strong></td>
            </tr>
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
            rab: {
                jasa: 0,
                material: 0
            },
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
                    value: 'jasas_sum'
                },
                {
                    text: 'RAB Material (Rp)',
                    value: 'materials_sum'
                },
            ],
        }
    },
    mounted() {
        this.data.forEach(el => {
            this.rab.jasa += el.jasas_sum;
            this.rab.material += el.materials_sum;
        });
    },
    updated() {
        let jasas = 0;
        let materials = 0;
        this.data.forEach(el => {
            jasas += el.jasas_sum;
            materials += el.materials_sum;
        });
        this.rab.jasa = jasas;
        this.rab.material = materials;
    },
}
</script>

<style>

</style>