<template>
    <v-row>
        <v-col cols="12">
            <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
            <div>
                <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
                <h2>Tahap Pelaksanaan</h2>
            </div>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Basket 1</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pelaksanaan-table 
                        v-if="!table.is_loading"
                        :basket="1" 
                        :data="table.basket_1" 
                        @show_detail="detail"
                    />
                    <div v-else >
                        <p>Sedang memuat...</p>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Basket 2</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pelaksanaan-table 
                        v-if="!table.is_loading"
                        :basket="2" 
                        :data="table.basket_2" 
                        @show_detail="detail"
                    />
                    <div v-else >
                        <p>Sedang memuat...</p>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Basket 3</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pelaksanaan-table 
                        v-if="!table.is_loading"
                        :basket="3" 
                        :data="table.basket_3" 
                        @show_detail="detail"
                    />
                    <div v-else >
                        <p>Sedang memuat...</p>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>
    </v-row>
</template>

<script>
import PelaksanaanTable from '~/components/table/PelaksanaanTable.vue';

export default {
    components: {
        PelaksanaanTable,
    },
    data() {
        return {
            breadcrumbItems: [
                {
                    text: 'Dashboard',
                    active: true,
                    href: '/'
                },
                {
                    text: 'Project Tahap Pelaksanaan',
                    active: false,
                },
            ],
            table: {
                is_loading: true,
                basket_1: [],
                basket_2: [],
                basket_3: []
            },
            form: {
                is_show: false,
                basket: '',
                data: ''
            }
        }
    },
    mounted() {
        this.load();
    },
    methods: {
        load() {
            this.$axios.get('/kontraks')
                .then(res => {
                    res.data.data.forEach(el => {
                        let sum_jasa = 0;
                        let sum_material = 0;

                        el.jasas.forEach(el => {
                            sum_jasa += el.harga;
                        })

                        el.materials.forEach(el => {
                            sum_material += el.harga * el.jumlah;
                        })

                        el.jasas_sum = sum_jasa;
                        el.materials_sum = sum_material;
                    })

                    this.table.basket_1 = res.data.data.filter(item => item.basket == 1)
                    this.table.basket_2 = res.data.data.filter(item => item.basket == 2)
                    this.table.basket_3 = res.data.data.filter(item => item.basket == 3)
                })
                .then(() => {
                    this.table.is_loading = false;
                })
        },
        detail(data) {
            this.form.data = data;
            this.form.is_show = true;
        },
    }
}
</script>

<style>

</style>
