<template>
    <div>
        <v-dialog v-model="is_show" fullscreen hide-overlay transition="dialog-bottom-transition">
            <v-card>
                <v-toolbar dark color="success">
                    <v-btn icon dark @click="$emit('hide_dialog')">
                        <v-icon>mdi-close</v-icon>
                    </v-btn>
                    <v-toolbar-title>Detail Kontrak</v-toolbar-title>
                    <v-spacer></v-spacer>
                    <v-toolbar-items>
                        <v-btn dark text @click="showAmandemenDialog"><v-icon>mdi-content-save</v-icon> Simpan</v-btn>
                    </v-toolbar-items>
                </v-toolbar>
                <v-card-text class="pt-6">
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <h2>Informasi Project</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Nomor Kontrak</label>
                                    <v-text-field solo dense v-model="new_project.no_prk" placeholder="Masukkan nomor kontrak"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Nama Project</label>
                                    <v-text-field solo dense v-model="new_project.no_prk" placeholder="Masukkan nama project"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Pelaksana</label>
                                    <v-text-field solo dense v-model="new_project.no_prk" placeholder="Masukkan pelaksana"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Direksi</label>
                                    <v-text-field solo dense v-model="new_project.no_prk" placeholder="Masukkan nama direksi"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pt-0">
                                    <label for="">Tanggal Awal - Akhir</label>
                                    <div>
                                        <strong>{{ date_picker.dates }}</strong> 
                                    </div>
                                    <div class="mb-4">
                                        <v-btn color="success" @click="openDatePicker">Pilih Tanggal</v-btn>
                                        <date-range-picker :is_show="date_picker.is_show" @save_date="saveDate" @hide_dialog="date_picker.is_show = false"/>
                                    </div>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pt-0">
                                    <label for="">Status</label>
                                    <v-select
                                        :items="['Proses', 'Terkontrak']"
                                        placeholder="Pilih status"
                                        solo
                                        dense
                                        v-model="detail_kontrak.status"
                                    ></v-select>
                                </v-col>
                            </v-row>
                            
                        </v-col>
                    </v-row>
                    <v-divider class="my-8"></v-divider>
                    <v-row class="mb-3">
                        <v-col cols="12">
                            <h2>RAB Jasa</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pb-0">
                                    <label for="">Nomor WBS Jasa</label>
                                    <v-select
                                        :items="['WBSJ1', 'WBSJ2', 'WBSJ3']"
                                        placeholder="Pilih PKR"
                                        solo
                                        dense
                                        v-model="detail_kontrak.wbsj"
                                    ></v-select>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col cols="12" class="mb-4">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="b-0">
                                    <label for="">Saldo</label>
                                    <h2>Rp.2.000.000.000,-</h2>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col cols="12" class="pt-0">
                            <label for="">List Jasa</label>
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
                                        <tr v-for="(item, index) in jasa" :key="index">
                                            <td>{{ item.nama }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.nilai) }},-</td>
                                            <td>
                                                <v-btn icon color="text--red" class="elevation-0" @click="deleteJasa(index)">
                                                    <v-icon color="red">mdi-delete</v-icon>
                                                </v-btn>
                                            </td>
                                        </tr>
                                        <tr v-if="is_edit.jasa">
                                            <td class="pt-4">
                                                <v-text-field solo dense placeholder="Masukkan nama jasa" v-model="new_jasa.nama"></v-text-field>
                                            </td>
                                            <td class="pt-4">
                                                <v-text-field solo dense placeholder="Masukkan nilai jasa (Rp)" type="number" v-model="new_jasa.nilai"></v-text-field>
                                            </td>
                                            <td class="d-flex align-start pt-4">
                                                <v-btn color="success" class="elevation-0" @click="saveJasa">+</v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
                    <v-divider class="my-8"></v-divider>
                    <v-row>
                        <v-col cols="12">
                            <h2>RAB Material</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="3" class="pb-0">
                                    <label for="">Nomor WBS Material</label>
                                    <v-select
                                        :items="['WBSM1', 'WBSM2', 'WBSM3']"
                                        placeholder="Pilih PKR"
                                        solo
                                        dense
                                        v-model="detail_kontrak.wbsm"
                                    ></v-select>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col cols="12" class="pt-0">
                            <label for="">List Material</label>
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
                                        <tr v-if="is_edit.material">
                                            <td class="pt-4">
                                                <v-text-field solo dense v-model="new_material.normalisasi" placeholder="Masukkan kode normalisasi"></v-text-field>
                                            </td>
                                            <td class="pt-4">
                                                <v-text-field solo dense v-model="new_material.nama" placeholder="Masukkan nama"></v-text-field>
                                            </td>
                                            <td class="pt-4">
                                                <v-text-field solo dense v-model="new_material.jumlah" placeholder="Masukkan jumlah"></v-text-field>
                                            </td>
                                            <td class="pt-4">
                                                <v-text-field solo dense v-model="new_material.satuan" placeholder="Masukkan satuan"></v-text-field>
                                            </td>
                                            <td class="pt-4">
                                                <v-text-field solo dense v-model="new_material.harga" placeholder="Masukkan harga (Rp)"></v-text-field>
                                            </td>
                                            <td class="d-flex align-start pt-4">
                                                <v-btn color="success" class="elevation-0" @click="saveMaterial">+</v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
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
    props: ['is_show'],
    data() {
        return {
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
            jasa: [],
            material: [],
            dummy_prk: [
                'PRK1', 'PRK2', 'PRK3', 'PRK4', 'PRK5', 'PRK6', 'PRK7', 'PRK8', 'PRK9',
            ]
        }
    },
    methods: {
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
    }
}
</script>

<style>

</style>