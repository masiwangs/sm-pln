<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div>
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap PRK</h2>
      </div>
    </v-col>

    <v-col cols="12">
        <v-card elevation="0" class="rounded-lg">
            <v-card-title class="d-flex justify-space-between">
                <div>Basket 1</div>
                <v-btn elevation="0" class="rounded-lg" v-on:click="newProject(1)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
            </v-card-title>
            <v-divider class="mx-4"></v-divider>
            <v-card-text>
                <prk-table 
                    v-if="!table_loading"
                    :basket="1" 
                    :data_table="data_basket_1" 
                    @show_detail="detailProject"
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
      <v-card elevation="0" class="rounded-lg">
            <v-card-title class="d-flex justify-space-between">
                <div>Basket 2</div>
                <v-btn elevation="0" class="rounded-lg" v-on:click="newProject(2)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
            </v-card-title>
            <v-divider class="mx-4"></v-divider>
            <v-card-text>
                <prk-table 
                    :basket="2" 
                    :data_table="data_basket_2" 
                    @show_detail="detailProject"
                    v-if="!table_loading"
                />
                <div v-else>
                    <p>Sedang memuat...</p>
                </div>
            </v-card-text>
        </v-card>
    </v-col>
    <v-col cols="12">
      <v-card elevation="0" class="rounded-lg">
            <v-card-title class="d-flex justify-space-between">
                <div>Basket 3</div>
                <v-btn elevation="0" class="rounded-lg" v-on:click="newProject(3)" color="primary"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
            </v-card-title>
            <v-divider class="mx-4"></v-divider>
            <v-card-text>
                <prk-table 
                    :basket="3" 
                    :data_table="data_basket_3" 
                    @show_detail="detailProject"
                    v-if="!table_loading"
                />
                <div v-else>
                    <p>Sedang memuat...</p>
                </div>
            </v-card-text>
        </v-card>
    </v-col>

    <v-col cols="12">
      
    </v-col>
    <v-col cols="12">
      
    </v-col>
    
    <material-list :is_show="material.is_show" :data="material.data" @hide_dialog="material.is_show = false"/>
    <jasa-list :is_show="jasa.is_show" :data="jasa.data" @hide_dialog="jasa.is_show = false"/>
    <prk-form 
        :is_show="prk_form.is_show" 
        :detail_project="detail_prk"
        :basket="prk_form.basket"
        @hide_dialog="prk_form.is_show = false"
        @reload_data="loadBasketData()"
    />
  </v-row>
</template>

<script>
    import BasketCard from '~/components/card/BasketCard.vue'
    import MaterialList from '~/components/dialog/MaterialList.vue'
    import JasaList from '~/components/dialog/JasaList.vue'
    import PrkForm from '~/components/dialog/PrkForm.vue'
    import PrkTable from '~/components/table/PrkTable.vue'
    export default {
        components: {BasketCard, MaterialList, JasaList, PrkForm, PrkTable},
        data() {
            return {
                breadcrumbItems: [
                    {
                        text: 'Dashboard',
                        active: true,
                        href: '/'
                    },
                    {
                        text: 'Project Tahap PRK',
                        active: false,
                    },
                ],
                table_loading: true,
                data_basket_1: [],
                data_basket_2: [],
                data_basket_3: [],

                detail_prk: '',

                prk_form: {
                    is_show: false,
                    basket: ''
                },

                material: {
                is_show: false,
                data: []
                },

                jasa: {
                    is_show: false,
                    data: []
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
            this.loadBasketData();
        },
        methods: {
            newProject(basket) {
                let form_data = new FormData();
                form_data.append('basket', basket)
                this.$axios.post('/prks', form_data)
                    .then(res => {
                        this.$router.push('/projects/prk/'+res.data.data.id)
                    })
            },
            loadBasketData() {
                this.$axios.get('/prks')
                    .then(res => {
                        // sum jasa
                        res.data.data.forEach(el => {
                            let sum_jasa = 0;
                            el.jasas.forEach(el => {
                                sum_jasa += el.harga
                            });
                            el.jasas_sum = sum_jasa

                            let sum_material = 0;
                            el.materials.forEach(el => {
                                sum_material += el.harga * el.jumlah
                            })
                            el.materials_sum = sum_material;
                        })

                        this.data_basket_1 = res.data.data.filter(item => item.basket == 1);
                        this.data_basket_2 = res.data.data.filter(item => item.basket == 2);
                        this.data_basket_3 = res.data.data.filter(item => item.basket == 3);

                        this.table_loading = false
                    })
                    .catch(error => {
                        console.log(error.response)
                    })
            },
            detailProject(data) {
                this.detail_prk = data
                this.prk_form.is_show = true
            },
            showMaterial(data) {
                this.material.data = data
                this.material.is_show = true
            },
            showJasa(data) {
                this.jasa.data = data
                this.jasa.is_show = true
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
            buatProjectBaru(basket) {
                this.project_baru_dialog.is_open = true
                this.project_baru_dialog.basket = basket
            },
            tambahMaterialProjectBaru() {
                this.project_baru_dialog.list_material.push(
                {
                    nama: this.material_project_baru.nama,
                    jumlah: this.material_project_baru.jumlah,
                    harga: this.material_project_baru.harga,
                }
                )
                this.material_project_baru.nama = ''
                this.material_project_baru.jumlah = ''
                this.material_project_baru.harga = ''
            },
            hapusMaterialProjectBaru(i) {
                this.project_baru_dialog.list_material = this.project_baru_dialog.list_material.filter((item, index) => index !== i)
            },
            tambahJasaProjectBaru() {
                this.project_baru_dialog.list_jasa.push(
                {
                    jasa: this.jasa_project_baru.jasa,
                    nilai: this.jasa_project_baru.nilai
                }
                )
                this.jasa_project_baru.jasa = ''
                this.jasa_project_baru.nilai = ''
            },
            hapusJasaProjectBaru(i) {
                this.project_baru_dialog.list_jasa = this.project_baru_dialog.list_jasa.filter((item, index) => index !== i)
            },

            increaseCount(n) {
                this.count += n
                console.log(this.count)
            }
        },
    }
</script>

<style>

</style>
