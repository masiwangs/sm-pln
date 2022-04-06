<template>
    <v-dialog v-model="is_show" fullscreen hide-overlay transition="dialog-bottom-transition">
        <v-card>
            <v-card-text class="pt-6">
                <v-container>
                    <!-- header start -->
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <p style="font-size: 2.5rem; line-height: 2.5rem">SKKI Baru Basket {{ new_skki.basket }}</p>
                            <v-chip
                                v-if="new_skki.last_modified"
                                small
                                dark
                                color="green accent-1"
                                class="green--text"
                            >Terakhir disimpan: {{ (new Date(new_skki.last_modified)).toLocaleString('id-ID') }}</v-chip>
                        </v-col>
                    </v-row>
                    <!-- heder end -->
                    <!-- informasi project start -->
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <h2>Informasi Project</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label for="">Nama Project</label>
                                    <v-select
                                        :items="prks.map((prk) => prk.nomor_prk)"
                                        placeholder="Pilih satu atau beberapa PKR"
                                        multiple
                                        chips
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        v-model="new_skki.prks"
                                        @change="saveSkki()"
                                    ></v-select>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Nomor SKKI</label>
                                    <v-text-field 
                                        filled
                                        rounded 
                                        dense 
                                        v-model="new_skki.nomor_skki" 
                                        :hide-details="true"
                                        placeholder="Masukkan nomor PRK"
                                        @change="saveSkki()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Nomor PRK-SKKI</label>
                                    <v-text-field 
                                        filled 
                                        rounded
                                        dense 
                                        v-model="new_skki.nomor_prk_skki" 
                                        :hide-details="true"
                                        placeholder="Masukkan nomor PRK-SKKI"
                                        @change="saveSkki()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                        </v-col>
                    </v-row>
                    <!-- informasi project end -->
                    <v-divider class="my-8"></v-divider>
                    <!-- rab jasa start -->
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <h2>RAB Jasa</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pb-0">
                                    <label for="">Nomor WBS Jasa</label>
                                    <v-text-field 
                                        filled
                                        rounded
                                        dense 
                                        v-model="new_skki.nomor_wbs_jasa" 
                                        placeholder="Masukkan nomor WBS jasa"
                                        @change="saveSkki()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col cols="12" class="pt-0">
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Jasa</th>
                                            <th class="text-left">Nilai</th>
                                            <th class="text-left" width="10%">Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in jasas" :key="item.id">
                                            <td>{{ item.nama_jasa }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-</td>
                                            <td>
                                                <v-btn icon color="text--red" class="elevation-0" @click="deleteJasa(item.id)">
                                                    <v-icon color="red">mdi-delete</v-icon>
                                                </v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
                    <!-- rab jasa end -->
                    <v-divider class="my-8"></v-divider>
                    <!-- rab material start -->
                    <v-row>
                        <v-col cols="12">
                            <h2>RAB Material</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pb-0">
                                    <label for="">Nomor WBS Material</label>
                                    <v-text-field 
                                        filled
                                        rounded 
                                        dense 
                                        v-model="new_skki.nomor_wbs_material" 
                                        placeholder="Masukkan nomor WBS material"
                                        @change="saveSkki()"    
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col cols="12" class="pt-0">
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Normalisasi</th>
                                            <th class="text-left">Nama</th>
                                            <th class="text-left">Jumlah</th>
                                            <th class="text-left">Satuan</th>
                                            <th class="text-left">Harga</th>
                                            <th class="text-left" width="10%">Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in materials" :key="item.id">
                                            <td>{{ item.kode_normalisasi }}</td>
                                            <td>{{ item.nama_material }}</td>
                                            <td>{{ item.jumlah }}</td>
                                            <td>{{ item.satuan }}</td>
                                            <td>{{ item.harga }}</td>
                                            <td>
                                                <v-btn icon color="text--red" class="elevation-0" @click="deleteMaterial(item.id)">
                                                    <v-icon color="red">mdi-delete</v-icon>
                                                </v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
                    <!-- rab material end -->
                    <v-divider class="my-8"></v-divider>
                    <!-- lampiran berkas start -->
                    <v-row>
                        <v-col cols="12" class="d-flex justify-space-between align-center">
                            <h2>Lampiran Berkas</h2>
                            <v-btn text color="success"><v-icon>mdi-upload</v-icon> Upload</v-btn>
                        </v-col>
                        <v-col cols="12">
                            <v-simple-table>
                                <template v-slot:default>
                                    <tbody>
                                        <tr>
                                            <td>Berkas_01_02.xlsx</td>
                                            <td class="text-right">
                                                <v-btn text small color="primary"><v-icon>mdi-download</v-icon> unduh</v-btn>
                                                <v-btn text small color="red"><v-icon>mdi-delete</v-icon></v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
                    <!-- lampiran berkas end -->
                    <v-divider class="my-8"></v-divider>
                    <!-- footer start -->
                    <div class="text-right">
                        <v-btn class="rounded-lg mr-2" elevation="0" dark @click="remove_dialog.is_show = true" color="red">Buang</v-btn>
                        <v-dialog
                            v-model="remove_dialog.is_show"
                            max-width="400"
                        >
                            <v-card
                                class="rounded-lg"
                            >
                                <v-card-title>Buang perubahan?</v-card-title>
                                <v-card-text>
                                    <v-container>
                                        <p class="mb-4">Data yang dibuang tidak dapat dikembalikan lagi. Lanjutkan?</p>
                                        <div
                                            class="text-right"
                                        >
                                            <v-btn
                                                light
                                                elevation="0"
                                                class="rounded-lg mr-2"
                                                @click="remove_dialog.is_show = false"
                                            >Batal</v-btn>
                                            <v-btn
                                                dark
                                                color="red"
                                                elevation="0"
                                                class="rounded-lg"
                                                @click="deleteSkki()"
                                            >Buang</v-btn>
                                        </div>
                                    </v-container>
                                </v-card-text>
                            </v-card>
                        </v-dialog>
                        <v-btn 
                            class="rounded-lg me-2" 
                            elevation="0" 
                            dark 
                            color="indigo accent-2"
                            @click="doneEditingSKKI()"
                        >Selesai</v-btn>
                    </div>
                </v-container>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: ['is_show', 'data', 'prks', 'basket'],
    data() {
        return {
            new_skki: {
                id: '',
                last_modified: '',
                prks: [],
                nomor_skki: '',
                nomor_prk_skki: '',
                nomor_wbs_jasa: '',
                nomor_wbs_material: '',
                basket: ''
            },

            jasas: [],
            materials: [],

            remove_dialog: {
                is_show: false
            },
            new_project: {
                nama: '',
                no_prk: '',
                lot_number: '',
                prioritas: 0
            },
            new_jasa: {
                nama: '',
                nilai: ''
            },
            new_material: {
                normalisasi: '',
                nama: '',
                jumlah: '',
                satuan: '',
                harga: ''
            },
            
            dummy_prk: [
                'PRK1', 'PRK2', 'PRK3', 'PRK4', 'PRK5', 'PRK6', 'PRK7', 'PRK8', 'PRK9',
            ]
        }
    },
    methods: {
        doneEditingSKKI(){
            this.new_skki.id = '';
            this.new_skki.last_modified = '';
            this.new_skki.prks = [];
            this.new_skki.nomor_skki = '';
            this.new_skki.nomor_prk_skki = '';
            this.new_skki.nomor_wbs_jasa = '';
            this.new_skki.nomor_wbs_material = '';
            this.$emit('reload_data');
            this.$emit('hide_dialog');
        },

        saveSkki(){
            let form_data = new FormData();
            form_data.append('nomor_skki', this.new_skki.nomor_skki)
            form_data.append('nomor_prk_skki', this.new_skki.nomor_prk_skki)
            form_data.append('nomor_wbs_jasa', this.new_skki.nomor_wbs_jasa)
            form_data.append('nomor_wbs_material', this.new_skki.nomor_wbs_material)
            form_data.append('prks', JSON.stringify(this.new_skki.prks))
            form_data.append('basket', this.new_skki.basket)

            if(this.new_skki.id && (this.new_skki.prks || this.new_skki.nomor_skki || this.new_skki.nomor_wbs_jasa || this.new_skki.nomor_wbs_material)) {
                this.$axios.post('/skkis/'+this.new_skki.id, form_data)
                    .then(res => {
                        this.new_skki.id = res.data.data.id;
                        this.new_skki.last_modified = res.data.data.updated_at
                        this.loadJasa();
                        this.loadMaterial()
                    })
            } else {
                this.$axios.post('/skkis', form_data)
                    .then(res => {
                        this.new_skki.id = res.data.data.id;
                        this.new_skki.last_modified = res.data.data.updated_at
                        this.loadJasa();
                        this.loadMaterial()
                    })
            }

        },

        loadJasa() {
            this.$axios.get('/skkis/'+this.new_skki.id+'/jasas')
                .then(res => {
                    this.jasas = res.data.data
                })
        },
        
        loadMaterial() {
            this.$axios.get('/skkis/'+this.new_skki.id+'/materials')
                .then(res => {
                    this.materials = res.data.data
                })
        },

        justLog() {
        console.log('log aja brou')
        },

        deleteJasa(id) {
            this.$axios.delete('/skkis/'+this.new_skki.id+'/jasas/'+id)
                .then(res => {
                    this.loadJasa()
                })
        },

        deleteMaterial(id) {
            this.$axios.delete('/skkis/'+this.new_skki.id+'/materials/'+id)
                .then(res => {
                    this.loadMaterial()
                })
        },

        deleteSkki() {
            this.$axios.delete('/skkis/'+this.new_skki.id)
                .then(res => {
                    this.$emit('reload_data')
                    this.$emit('hide_dialog')
                })
        }
    },
    watch: {
        basket(val) {
            this.new_skki.basket = val
        },
        data(data) {
            this.new_skki.id = data.id;
            this.new_skki.last_modified = data.updated_at;
            this.new_skki.prks = JSON.parse(data.prks);
            this.new_skki.nomor_skki = data.nomor_skki;
            this.new_skki.nomor_prk_skki = data.nomor_prk_skki;
            this.new_skki.nomor_wbs_jasa = data.nomor_wbs_jasa;
            this.new_skki.nomor_wbs_material = data.nomor_wbs_material;
            this.new_skki.basket = data.basket;

            this.loadJasa();
            this.loadMaterial();
        }
    }
}
</script>

<style>

</style>