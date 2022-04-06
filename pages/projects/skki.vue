<template>
    <v-row>
        <v-col cols="12">
          <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
          <div>
            <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
            <h2>Tahap SKKI</h2>
          </div>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
            <v-card-title class="d-flex justify-space-between">
                <div>Basket 1</div>
                <v-btn elevation="0" class="rounded-lg" v-on:click="newSKKI(1)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
            </v-card-title>
            <v-divider class="mx-4"></v-divider>
            <v-card-text>
                <skki-table 
                    v-if="!table_loading"
                    :basket="1" 
                    :data="data_skki_basket_1" 
                    @show_detail="detailSkki"
                />
                <div
                    v-else
                >
                    <p>Sedang memuat...</p>
                </div>
            </v-card-text>
        </v-card>
        </v-col>
        <v-col cols="12">
          <basket-card tahap="skki" :basket="2" @show_material="showMaterial" @show_jasa="showJasa" @new_project="newProject"/>
        </v-col>
        <v-col cols="12">
          <basket-card tahap="skki" :basket="3" @show_material="showMaterial" @show_jasa="showJasa" @new_project="newProject"/>
        </v-col>
        
        <material-list :is_show="material.is_show" :data="material.data" @hide_dialog="material.is_show = false"/>
        <jasa-list :is_show="jasa.is_show" :data="jasa.data" @hide_dialog="jasa.is_show = false"/>
        <skki-form 
            :is_show="skki_form.is_show" 
            :basket="skki_form.basket"
            :data="skki_form.data"
            :prks="skki_form.prks"
            @hide_dialog="skki_form.is_show = false"
            @reload_data="loadData()"
        />
    </v-row>
</template>

<script>
    import BasketCard from '~/components/card/BasketCard.vue'
    import MaterialList from '~/components/dialog/MaterialList.vue'
    import JasaList from '~/components/dialog/JasaList.vue'
    import SkkiForm from '~/components/dialog/SkkiForm.vue'
    import SkkiTable from '~/components/table/SkkiTable.vue'

    export default {
        components: {BasketCard, MaterialList, JasaList, SkkiForm, SkkiTable},
        data() {
        return {
            breadcrumbItems: [
                {
                    text: 'Dashboard',
                    active: true,
                    href: '/'
                },
                {
                    text: 'Project Tahap SKKI',
                    active: false,
                },
            ],
            skki_form: {
                is_show: false,
                basket: '',
                data: '',
                prks: []
            },
            table_loading: true,
            data_skki_basket_1: [],

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
            this.loadData();
        },
        methods: {
            loadData() {
                this.$axios.get('/skkis')
                    .then(res => {
                        this.data_skki_basket_1 = res.data.data.filter(item => item.basket == 1)
                        this.table_loading = false
                    })
            },
            newSKKI(basket) {
                this.$axios.get('/prks')
                    .then(res => {
                        this.skki_form.prks = res.data.data
                    })
                this.skki_form.basket = basket
                this.skki_form.is_show = true
            },
            detailSkki(data) {
                this.$axios.get('/prks')
                    .then(res => {
                        this.skki_form.prks = res.data.data;
                        this.skki_form.data = data;
                        this.skki_form.is_show = true;
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
