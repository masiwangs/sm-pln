<template>
    <v-dialog v-model="is_show" fullscreen hide-overlay transition="dialog-bottom-transition">
        <v-card>
            <v-card-text class="pt-6">
                <v-container>
                    <!-- header start -->
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <p style="font-size: 2.5rem; line-height: 2.5rem">Project Baru Basket {{ new_project.basket }}</p>
                            <v-chip 
                                v-if="new_project.last_modified"
                                small
                                dark
                                color="green accent-1"
                                class="green--text"
                            >Terakhir disimpan: {{ (new Date(new_project.last_modified)).toLocaleString('id-ID') }}</v-chip>
                        </v-col>
                    </v-row>
                    <!-- header end -->
                    <!-- informasi project start -->
                    <v-row>
                        <v-col cols="12" class="mb-4">
                            <h2>Informasi Project</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label for="">Nama Project</label>
                                    <v-text-field 
                                        filled 
                                        rounded 
                                        :hide-details="true" 
                                        dense 
                                        v-model="new_project.nama" 
                                        @change="savePrk()"
                                        placeholder="Masukkan nama project"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label for="">Nomor PRK</label>
                                    <v-text-field 
                                        filled 
                                        rounded 
                                        :hide-details="true" 
                                        dense 
                                        v-model="new_project.no_prk" 
                                        @change="savePrk()"
                                        placeholder="Masukkan nomor PRK"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label for="">Lot Number</label>
                                    <v-text-field 
                                        filled 
                                        rounded 
                                        :hide-details="true" 
                                        dense 
                                        v-model="new_project.lot_number" 
                                        @change="savePrk()"
                                        placeholder="Masukkan lot number"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label>Prioritas</label>
                                    <div class="d-flex align-center">
                                        <v-rating
                                            v-model="prioritas"
                                            length="4"
                                            background-color="orange lighten-3"
                                            color="orange"
                                            hover
                                            large
                                        ></v-rating>
                                        <h4 class="mr-3">( {{ new_project.prioritas }} )</h4>( <span role="button" class="primary--text" @click="prioritas = 0"><u>reset</u></span> )
                                    </div>
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
                        <v-col cols="12" class="d-flex flex-row">
                            <div class="me-2">
                                <v-btn @click="new_jasa_dialog.is_show = true" dark class="rounded-lg" elevation="0" color="indigo accent-2"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
                                <!-- jasa baru input manual start -->
                                <v-dialog
                                    max-width="400px"
                                    v-model="new_jasa_dialog.is_show"
                                    persistent
                                >
                                    <v-card class="rounded-lg">
                                        <v-card-title>Jasa Baru</v-card-title>
                                        <v-card-text>
                                            <v-container>
                                                <v-form>
                                                    <label for="">Nama Jasa</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        class="mb-3"
                                                        placeholder="Masukkan nama jasa"
                                                        v-model="new_jasa_dialog.data.nama"
                                                    ></v-text-field>
                                                    <label for="">Nilai</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        type="number"
                                                        class="mb-4"
                                                        placeholder="Masukkan nilai dalam Rp"
                                                        v-model="new_jasa_dialog.data.nilai"
                                                    ></v-text-field>
                                                    <div
                                                        class="text-right"
                                                    >
                                                        <v-btn 
                                                            dark 
                                                            color="red" 
                                                            class="rounded-lg" 
                                                            elevation="0"
                                                            @click="cancelNewJasa()"
                                                        >Batal</v-btn>
                                                        <v-btn 
                                                            dark 
                                                            color="indigo accent-2" 
                                                            class="rounded-lg" 
                                                            elevation="0"
                                                            @click="saveNewJasa()"
                                                        >Simpan</v-btn>
                                                    </div>
                                                </v-form>
                                            </v-container>
                                        </v-card-text>
                                    </v-card>
                                </v-dialog>
                                <!-- jasa baru input manual end -->
                            </div>
                            <div>
                                <v-btn 
                                    dark 
                                    class="rounded-lg" 
                                    elevation="0" 
                                    color="green dark-3"
                                    @click="jasa_import_dialog.is_show = true"
                                >
                                    <v-icon class="mr-2">mdi-microsoft-excel</v-icon> Import .xlsx
                                </v-btn>
                                <v-dialog
                                    v-model="jasa_import_dialog.is_show"
                                    persistent
                                    max-width="400"
                                >
                                    <v-card 
                                        class="rounded-lg"
                                        elevation="0"
                                    >
                                        <v-card-title>Import dari Excel</v-card-title>
                                        <v-card-text>
                                            <v-container>
                                                <v-form>
                                                    <label for="">Pilih file</label>
                                                    <v-file-input
                                                        filled
                                                        rounded
                                                        dense
                                                        hint="Hanya file .xlsx"
                                                        persistent-hint
                                                        class="mb-4"
                                                    ></v-file-input>
                                                    <div class="text-right">
                                                        <v-btn
                                                            dark
                                                            color="red"
                                                            class="rounded-lg"
                                                            elevation="0"
                                                            @click="jasa_import_dialog.is_show = false"
                                                        >Batal</v-btn>
                                                        <v-btn
                                                            dark
                                                            color="indigo accent-2"
                                                            class="rounded-lg"
                                                            elevation="0"
                                                        >Simpan</v-btn>
                                                    </div>
                                                </v-form>
                                            </v-container>
                                        </v-card-text>
                                    </v-card>
                                </v-dialog>
                                <div class="text-right">
                                    <small><v-icon small>mdi-help-circle</v-icon> Bantuan</small>
                                </div>
                            </div>
                        </v-col>
                        <v-col cols="12">
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
                                        <tr v-for="item in jasa" :key="item.id">
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
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <h2>RAB Material</h2>
                        </v-col>
                        <v-col cols="12" class="d-flex flex-row">
                            <div class="me-2">
                                <v-btn @click="new_material_dialog.is_show = true" dark class="rounded-lg" elevation="0" color="indigo accent-2"><v-icon class="mr-2">mdi-plus-box</v-icon> Baru</v-btn>
                                <!-- material baru input manual start -->
                                <v-dialog
                                    max-width="400px"
                                    v-model="new_material_dialog.is_show"
                                    persistent
                                >
                                    <v-card class="rounded-lg">
                                        <v-card-title>Material Baru</v-card-title>
                                        <v-card-text>
                                            <v-container>
                                                <v-form>
                                                    <label for="">Kode Normalisasi</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        class="mb-3"
                                                        placeholder="Masukkan kode normalisasi"
                                                        v-model="new_material_dialog.data.normalisasi"
                                                    ></v-text-field>
                                                    <label for="">Nama Material</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        class="mb-3"
                                                        placeholder="Masukkan nama material"
                                                        v-model="new_material_dialog.data.nama"
                                                    ></v-text-field>
                                                    <label for="">Satuan</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        class="mb-3"
                                                        placeholder="Masukkan satuan material"
                                                        v-model="new_material_dialog.data.satuan"
                                                    ></v-text-field>
                                                    <label for="">Jumlah</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        class="mb-3"
                                                        placeholder="Masukkan jumlah material"
                                                        v-model="new_material_dialog.data.jumlah"
                                                        :suffix="new_material_dialog.data.satuan"
                                                    ></v-text-field>
                                                    
                                                    <label for="">Harga</label>
                                                    <v-text-field
                                                        filled
                                                        rounded
                                                        :hide-details="true"
                                                        dense
                                                        type="number"
                                                        class="mb-4"
                                                        placeholder="Masukkan harga material"
                                                        :suffix="'/'+new_material_dialog.data.satuan"
                                                        v-model="new_material_dialog.data.harga"
                                                    ></v-text-field>
                                                    <div
                                                        class="text-right"
                                                    >
                                                        <v-btn 
                                                            dark 
                                                            color="red" 
                                                            class="rounded-lg" 
                                                            elevation="0"
                                                            @click="cancelNewMaterial()"
                                                        >Batal</v-btn>
                                                        <v-btn 
                                                            dark 
                                                            color="indigo accent-2" 
                                                            class="rounded-lg" 
                                                            elevation="0"
                                                            @click="saveNewMaterial()"
                                                        >Simpan</v-btn>
                                                    </div>
                                                </v-form>
                                            </v-container>
                                        </v-card-text>
                                    </v-card>
                                </v-dialog>
                                <!-- jasa baru input manual end -->
                            </div>
                            <div>
                                <v-btn 
                                    dark 
                                    class="rounded-lg" 
                                    elevation="0" 
                                    color="green dark-3"
                                    @click="jasa_import_dialog.is_show = true"
                                >
                                    <v-icon class="mr-2">mdi-microsoft-excel</v-icon> Import .xlsx
                                </v-btn>
                                <v-dialog
                                    v-model="jasa_import_dialog.is_show"
                                    persistent
                                    max-width="400"
                                >
                                    <v-card 
                                        class="rounded-lg"
                                        elevation="0"
                                    >
                                        <v-card-title>Import dari Excel</v-card-title>
                                        <v-card-text>
                                            <v-container>
                                                <v-form>
                                                    <label for="">Pilih file</label>
                                                    <v-file-input
                                                        filled
                                                        rounded
                                                        dense
                                                        hint="Hanya file .xlsx"
                                                        persistent-hint
                                                        class="mb-4"
                                                    ></v-file-input>
                                                    <div class="text-right">
                                                        <v-btn
                                                            dark
                                                            color="red"
                                                            class="rounded-lg"
                                                            elevation="0"
                                                            @click="jasa_import_dialog.is_show = false"
                                                        >Batal</v-btn>
                                                        <v-btn
                                                            dark
                                                            color="indigo accent-2"
                                                            class="rounded-lg"
                                                            elevation="0"
                                                        >Simpan</v-btn>
                                                    </div>
                                                </v-form>
                                            </v-container>
                                        </v-card-text>
                                    </v-card>
                                </v-dialog>
                                <div class="text-right">
                                    <small><v-icon small>mdi-help-circle</v-icon> Bantuan</small>
                                </div>
                            </div>
                        </v-col>
                        <v-col cols="12">
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
                                        <tr v-for="(item, index) in material" :key="index">
                                            <td>{{ item.normalisasi }}</td>
                                            <td>{{ item.nama }}</td>
                                            <td>{{ item.jumlah }}</td>
                                            <td>{{ item.satuan }}</td>
                                            <td>{{ item.harga }}</td>
                                            <td>
                                                <v-btn icon color="text--red" class="elevation-0" @click="deleteMaterial(index)">
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
                    <v-divider class="my-8"></v-divider>
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
                                                @click="deletePrk()"
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
                            @click="doneEditingPrk()"
                        >Selesai</v-btn>
                    </div>
                </v-container>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: ['is_show', 'detail_project'],
    data() {
        return {
            new_jasa_dialog: {
                is_show: false,
                data: {
                    nama: '',
                    nilai: ''
                }
            },
            new_material_dialog: {
                is_show: false,
                data: {
                    normalisasi: '',
                    nama: '',
                    jumlah: 0,
                    satuan: '',
                    harga: 0
                }
            },
            remove_dialog: {
                is_show: false
            },
            jasa_import_dialog: {
                is_show: false,
            },
            prioritas: 0,
            new_project: {
                id: '',
                nama: '',
                no_prk: '',
                lot_number: '',
                prioritas: 0,
                last_modified: '',
                basket: ''
            },
            jasa: [],
            material: []
        }
    },
    methods: {
        savePrk() {
            let form_data = new FormData();
            form_data.append('nama_project', this.new_project.nama)
            form_data.append('nomor_prk', this.new_project.no_prk)
            form_data.append('lot_number', this.new_project.lot_number)
            form_data.append('prioritas', this.new_project.prioritas)
            form_data.append('project_id', 1)
            form_data.append('basket', this.new_project.basket)
            if(this.new_project.id) {
                this.$axios.post('/prks/'+this.new_project.id, form_data)
                    .then(res => {
                        this.new_project.id = res.data.data.id
                        this.new_project.last_modified = res.data.data.updated_at
                    })
            } else {
                this.$axios.post('/prks', form_data)
                    .then(res => {
                        this.new_project.id = res.data.data.id
                        this.new_project.last_modified = res.data.data.updated_at
                    })
            }
            console.log(this.new_project)
        },
        deletePrk() {
            this.$axios.delete('/prks/'+this.new_project.id)
                .then(res => {
                    this.$emit('reload_data')
                    this.$emit('hide_dialog')
                })
        },
        doneEditingPrk() {
            this.$emit('reload_data')
            this.$emit('hide_dialog')
        },
        cancelNewJasa() {
            this.new_jasa_dialog.data.nama = ''
            this.new_jasa_dialog.data.nilai = ''
            this.new_jasa_dialog.is_show = false
        },
        cancelNewMaterial() {
            this.new_material_dialog.data.normalisasi = ''
            this.new_material_dialog.data.nama = ''
            this.new_material_dialog.data.jumlah = 0
            this.new_material_dialog.data.satuan = ''
            this.new_material_dialog.data.harga = 0
            this.new_material_dialog.is_show = false
        },
        saveNewJasa() {
            let form_data = new FormData();
            form_data.append('nama_jasa', this.new_jasa_dialog.data.nama)
            form_data.append('harga', this.new_jasa_dialog.data.nilai)
            this.$axios.post('/prks/'+this.new_project.id+'/jasas', form_data)
                .then(res => {
                    this.loadJasa();
                    this.new_jasa_dialog.is_show = false
                })
        },
        loadJasa(){
            this.$axios.get('/prks/'+this.new_project.id+'/jasas')
                .then(res => {
                    this.jasa = res.data.data
                })
        },
        saveMaterial() {
            this.material.push(this.new_material);
            this.new_material = {
                normalisasi: '',
                nama: '',
                jumlah: '',
                satuan: '',
                harga: ''
            }
        },
        deleteJasa(id) {
            this.$axios.delete('/prks/'+this.new_project.id+'/jasas/'+id)
                .then(res => {
                    this.loadJasa()
                })
        },
        deleteMaterial(index) {
            this.material.splice(index, 1)
        }
    },
    watch: {
        prioritas(val) {
            this.new_project.prioritas = val
            this.savePrk()
        },
        detail_project(val) {
            this.new_project.id = val.id
            this.new_project.nama = val.nama_project
            this.new_project.no_prk = val.nomor_prk
            this.new_project.lot_number = val.lot_number
            this.new_project.prioritas = val.prioritas
            this.prioritas = val.prioritas
            this.new_project.last_modified = val.updated_at
            this.new_project.basket = val.basket
            this.loadJasa()
        }
    }
}
</script>

<style>

</style>