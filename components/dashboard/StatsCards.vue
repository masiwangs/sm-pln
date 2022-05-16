<template>
  <v-row>
    <v-col v-for="(item, index) in label" :key="index" :detail="false" cols="12" md="6" lg="2" style="color: white">
        <stat 
            :label="item" 
            :color="color[index]" 
            :icon="icon[index]" 
            :dataJasa="dataJasa[index]" 
            :dataMaterial="dataMaterial[index]"
            :detail="false"
        />
    </v-col>
  </v-row>
</template>

<script>
import Stat from '~/components/dashboard/Stat.vue';
export default {
    components: {Stat},
    props: ['data'],
    data() {
        return {
            label: ['PRK', 'SKKI', 'Pengadaan', 'Kontrak', 'Pelaksanaan', 'Pembayaran'],
            color: ['primary', 'red', 'amber', 'success', 'purple', 'secondary'],
            icon: ['puzzle', 'note-text', 'cart', 'file-sign', 'crane', 'currency-usd'],
        }
    },
    computed: {
        dataJasa: function() {
        return this.data.filter((v, i) => i%2 == 0)
        },
        dataMaterial: function() {
        return this.data.filter((v, i) => i%2 == 1)
        }
    },
}
</script>

<style>

</style>