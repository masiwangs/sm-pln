<template>
    <v-row>
        <v-col cols="12">
        <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
        <div>
            <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
            <h2>Tahap Pengadaan</h2>
        </div>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Basket 1</div>
                    <v-btn elevation="0" class="rounded-lg" v-on:click="create(1)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pengadaan-table 
                        v-if="!table_loading"
                        :basket="1" 
                        :data="data_basket_1" 
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
                    <v-btn elevation="0" class="rounded-lg" v-on:click="create(2)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pengadaan-table 
                        v-if="!table_loading"
                        :basket="2" 
                        :data="data_basket_2" 
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
                    <v-btn elevation="0" class="rounded-lg" v-on:click="create(3)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <pengadaan-table 
                        v-if="!table_loading"
                        :basket="3" 
                        :data="data_basket_3" 
                        @show_detail="detail"
                    />
                    <div v-else >
                        <p>Sedang memuat...</p>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>
    
        <pengadaan-form 
            :is_show="form.is_show" 
            :basket="form.basket" 
            :data="form.data"
            :skkis="form.skkis"
            @hide_dialog="form.is_show = false"
            @reload_data="load()"/>
    </v-row>
</template>

<script>
    import BasketCard from '~/components/card/BasketCard.vue'
    import MaterialList from '~/components/dialog/MaterialList.vue'
    import JasaList from '~/components/dialog/JasaList.vue'
    import PengadaanForm from '~/components/dialog/PengadaanForm.vue'
    import PengadaanTable from '~/components/table/PengadaanTable.vue'

    export default {
        components: {BasketCard, MaterialList, JasaList, PengadaanForm, PengadaanTable},
        data() {
        return {
            breadcrumbItems: [
                {
                    text: 'Dashboard',
                    active: true,
                    href: '/'
                },
                {
                    text: 'Project Tahap Pengadaan',
                    active: false,
                },
            ],
            form:{
                is_show: false,
                basket: '',
                data: '',
                skkis: []
            },
            data_basket_1: [],
            data_basket_2: [],
            data_basket_3: [],
            table_loading: true,

            material: {
            is_show: false,
            data: []
            },

            jasa: {
            is_show: false,
            data: []
            },

            new_project: {
            is_show: false,
            basket: ''
            },

            jasa_dialog: false,
            material_dialog: false,

            detail_jasa_value: {
            nama_project: '',
            data: [],
            loading: true
            },
            detail_material_value: {
            nama_project: '',
            data: [],
            loading: true
            },
            project_baru_dialog: {
            is_open: false,
            basket: 1,
            list_jasa: [],
            list_material: []
            },
            material_project_baru: {
            nama: '',
            jumlah: '',
            harga: ''
            },
            jasa_project_baru: {
            jasa: '',
            nilai: ''
            },
            count: 0
        }
        },
        mounted() {
            this.load();
        },
        methods: {
            load(){
                this.data_basket = [];
                this.$axios.get('/pengadaans')
                    .then(res => {
                        // TODO: sum jasa dan material
                        res.data.data.forEach(el => {
                            let sum_jasa = 0;
                            let sum_material = 0;

                            el.jasas.forEach(el => {
                                sum_jasa += el.harga;
                            })

                            el.materials.forEach(el => {
                                sum_material += el.harga * el.jumlah
                            });

                            el.jasas_sum = sum_jasa;
                            el.materials_sum = sum_material;
                        })
                        
                        this.data_basket_1 = res.data.data.filter(item => item.basket == 1);
                        this.data_basket_2 = res.data.data.filter(item => item.basket == 2);
                        this.data_basket_3 = res.data.data.filter(item => item.basket == 3);
                    })
                    .then(() => {
                        this.table_loading = false;
                    })
            },
            create(basket){
                let form_data = new FormData();
                form_data.append('basket', basket)
                this.$axios.post('/pengadaans', form_data)
                    .then(res => {
                        this.$router.push('/projects/pengadaan/'+res.data.data.id)
                    })
            },
            detail(data){
                this.$axios.get('/skkis')
                    .then(res => {
                        this.form.skkis = res.data.data;
                        this.form.data = data;
                    })
                    .then(() => {
                        this.form.is_show = true;
                    })
            },
        showMaterial(data) {
            this.material.data = data
            this.material.is_show = true
        },
        showJasa(data) {
            this.jasa.data = data
            this.jasa.is_show = true
        },
        newProject(basket) {
            this.new_project.basket = basket
            this.new_project.is_show = true
            this.$refs.formSkki.justLog()
        },





        detailJasa(id, nama_project) {
            this.detail_jasa_value.nama_project = nama_project
            this.jasa_dialog = true
            this.detail_jasa_value.loading = true
            setTimeout(() => {
            this.detail_jasa_value.data = [{
                nama: 'Jasa A',
                nilai: Math.ceil(Math.random(1, 9)*10)*1000000
                },
                {
                nama: 'Jasa B',
                nilai: Math.ceil(Math.random(1, 20)*10)*100000
                },
            ]
            this.detail_jasa_value.loading = false
            }, 1000);
        },
        detailMaterial(id, nama_project) {
            this.detail_material_value.nama_project = nama_project
            this.material_dialog = true
            this.detail_material_value.loading = true
            setTimeout(() => {
            let jumlahA = Math.ceil(Math.random(1, 9) * 100);
            let jumlahB = Math.ceil(Math.random(1, 9) * 100);

            let hargaA = Math.ceil(Math.random(1, 9)*10)*1000000;
            let hargaB = Math.ceil(Math.random(1, 9)*10)*1000000;

            this.detail_material_value.data = [{
                nama: 'Material A',
                jumlah: jumlahA,
                harga: hargaA,
                total: jumlahA*hargaA
                },
                {
                nama: 'Material B',
                jumlah: jumlahB,
                harga: hargaB,
                total: jumlahB*hargaB
                },
            ]
            this.detail_material_value.loading = false
            }, 1000);
        },
        },
  }
</script>

<style>

</style>
