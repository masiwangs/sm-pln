<template>
    <div>
        <v-dialog 
            v-model="is_show" 
            fullscreen 
            hide-overlay 
            transition="dialog-bottom-transition"
        >
            <v-card>
                <v-card-text class="pt-6">
                    <v-container>
                        <!-- header start -->
                        <v-row class="mb-3">
                            <v-col cols="12">
                                <p
                                    style="
                                        font-size: 2.5rem;
                                        line-height: 2.5rem;
                                    "
                                >
                                    Detail Kontrak Basket {{ form.basket }}
                                </p>
                            </v-col>
                        </v-row>
                        <!-- header end -->
                        <v-row class="mb-3">
                            <v-col cols="12">
                                <h2>Informasi Project</h2>
                            </v-col>
                            <v-col cols="12">
                                <v-row>
                                    <v-col 
                                        cols="12" 
                                        md="6" 
                                        lg="4" 
                                        class="pt-0"
                                    >
                                        <label for="">Nomor Kontrak</label>
                                        <v-text-field 
                                            filled
                                            rounded 
                                            dense 
                                            v-model="form.nomor_kontrak" 
                                            :hide-details="true"
                                            placeholder="Masukkan nomor kontrak"></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col 
                                        cols="12" 
                                        md="6" 
                                        lg="4" 
                                        class="pt-0"
                                    >
                                        <label for="">Pelaksana</label>
                                        <v-text-field 
                                            filled 
                                            rounded
                                            dense 
                                            :hide-details="true"
                                            v-model="form.pelaksana" 
                                            placeholder="Masukkan pelaksana"></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col 
                                        cols="12" 
                                        md="6" 
                                        lg="4" 
                                        class="pt-0"
                                    >
                                        <label for="">Direksi</label>
                                        <v-text-field 
                                            filled
                                            rounded 
                                            dense 
                                            :hide-details="true"
                                            v-model="form.direksi_pelaksana" 
                                            placeholder="Masukkan nama direksi"></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col
                                        cols="12"
                                        md="6"
                                        lg="4"
                                        class="pt-0"
                                    >
                                        <label for="">Tanggal Kontrak</label>
                                        <v-text-field
                                            filled
                                            rounded
                                            dense
                                            :hide-details="true"
                                            v-model="form.tanggal_kontrak"
                                            type="date"
                                            placeholder="Pilih tanggal kontrak"
                                        ></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col
                                        cols="12"
                                        md="6"
                                        lg="4"
                                        class="pt-0"
                                    >
                                        <label for="">Tanggal Awal</label>
                                        <v-text-field
                                            filled
                                            rounded
                                            dense
                                            v-model="form.tanggal_awal"
                                            :hide-details="true"
                                            placeholder="Pilih tanggal awal"
                                            type="date"
                                        ></v-text-field>
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col
                                        cols="12"
                                        md="6"
                                        lg="4"
                                        class="pt-0"
                                    >
                                        <label for="">Tanggal Akhir</label>
                                        <v-text-field
                                            filled
                                            rounded
                                            dense
                                            v-model="form.tanggal_akhir"
                                            :hide-details="true"
                                            placeholder="Pilih tanggal akhir"
                                            type="date"
                                        ></v-text-field>
                                    </v-col>
                                </v-row>
                                
                            </v-col>
                        </v-row>
                        <v-divider class="my-8"></v-divider>
                        <!-- rab jasa start -->
                        <v-row class="mb-3">
                            <v-col cols="12">
                                <h2>Jasa</h2>
                            </v-col>
                            <v-col cols="12" class="pt-0">
                                <v-simple-table>
                                    <template v-slot:default>
                                        <thead>
                                            <tr>
                                                <th class="text-left">Jasa</th>
                                                <th class="text-left">Nilai</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="item in jasas" :key="item.id">
                                                <td>{{ item.nama_jasa }}</td>
                                                <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-</td>
                                                
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
                                <h2>Material</h2>
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
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="item in materials" :key="item.id">
                                                <td>{{ item.kode_normalisasi }}</td>
                                                <td>{{ item.nama_material }}</td>
                                                <td>{{ item.jumlah }}</td>
                                                <td>{{ item.satuan }}</td>
                                                <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-/{{ item.satuan }}</td>
                                                
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
                                                    @click="remove()"
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
                                @click="done()"
                            >Selesai</v-btn>
                        </div>
                    </v-container>
                </v-card-text>
            </v-card>
        </v-dialog>
        <v-dialog v-model="save_amandemen.is_show" max-width="240" persistent>
            <v-card>
                <v-card-title>Simpan</v-card-title>
                <v-card-text>
                    Simpan sebagai <strong>amandemen</strong>?
                </v-card-text>
                <v-card-actions class="text-right">
                    <v-btn @click="save_amandemen.is_show = false">Tidak</v-btn>
                    <v-btn color="success" @click="save_amandemen.is_show = false">Ya</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
import DateRangePicker from './DateRangePicker.vue'
export default {
    components: { DateRangePicker },
    props: ['is_show', 'data', 'basket'],
    data() {
        return {
            form: {
                id: '',
                basket: '',
                nomor_kontrak: '',
                tanggal_kontrak: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                pelaksana: '',
                direksi_pelaksana: '',
                pengadaan_id: '',
                is_amandemen: '',
                versi_amandemen: '',
                amandemen_id: '',
            },
            jasas: [],
            materials: [],
            remove_dialog: {
                is_show: false,
            },
            //-=========
            save_amandemen: {
                is_show: false
            },
            detail_kontrak: {
                status: '',
                wbsj: '',
                wbsm: ''
            },
            date_picker: {
                is_show: false,
                dates: []
            },
            new_project: {
                nama: '',
                no_prk: '',
                lot_number: '',
                prioritas: 0
            },
            is_edit: {
                jasa: false,
                material: false
            },
            edit_jasa: {
                nama: '',
                nilai: ''
            },
            edit_material: {
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
        done(){
            this.form = {
                id: '',
                basket: '',
                nomor_kontrak: '',
                tanggal_kontrak: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                pelaksana: '',
                direksi_pelaksana: '',
                pengadaan_id: '',
                is_amandemen: '',
                versi_amandemen: '',
                amandemen_id: '',
                wbs_jasas_option: [],
                nomor_wbs_jasas: [],
                wbs_materials_option: [],
                nomor_wbs_materials: ''
            };
            this.$emit('reload_data')
            this.$emit('hide_dialog')
        },
        // =====
        showAmandemenDialog() {
            this.save_amandemen.is_show = true
        },
        openDatePicker() {
            this.date_picker.is_show = true
        },
        saveDate(dates) {
            this.date_picker.dates = dates
        },
        justLog() {
            console.log('log aja brou')
        },
        saveJasa() {
            this.jasa.push(this.new_jasa)
            this.new_jasa = {
                nama: '',
                nilai: ''
            }
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
        deleteJasa(index) {
            this.jasa.splice(index, 1)
        },
        deleteMaterial(index) {
            this.material.splice(index, 1)
        }
    },
    watch: {
        data(data) {
            this.form = data
            this.jasas = data.jasas
            this.materials = data.materials
            console.log('in form', data)
            // this.loadWbsOption()
        }
    }
}
</script>

<style>

</style>