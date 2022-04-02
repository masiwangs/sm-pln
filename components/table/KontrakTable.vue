<template>
    <div>
        <v-simple-table>
            <template v-slot:default>
                <thead>
                    <tr>
                        <th>Nomor Kontrak</th>
                        <th>Nama Project</th>
                        <th>Tgl Awal</th>
                        <th>Tgl Akhir</th>
                        <th>Pelaksana</th>
                        <th>Direksi</th>
                        <th>RAB Jasa</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><a href="#" @click="showDetail">KTR12</a></td>
                        <td>ABC</td>
                        <td>2022-04-01</td>
                        <td>2022-05-01</td>
                        <td>PT XYZ</td>
                        <td>DEF</td>
                        <td>Rp{{ new Intl.NumberFormat('id-ID').format(2000000) }}</td>
                    </tr>
                </tbody>
            </template>
        </v-simple-table>

        <kontrak-form :is_show="detail.is_show" @hide_dialog="detail.is_show = false"/>
    </div>
</template>

<script>
import KontrakForm from '../dialog/KontrakForm.vue'
export default {
    props: ['data'],
    components: { KontrakForm },
    data() {
        return {
            detail: {
                is_show: false
            }
        }
    },
    methods: {
        showDetail() {
            this.detail.is_show = true
        }
    }
}
</script>

<style>

</style>