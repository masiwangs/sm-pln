<template>
    <v-dialog v-model="is_show" fullscreen hide-overlay transition="dialog-bottom-transition">
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
                                Pengadaan Baru Basket {{ form.basket }}
                            </p>
                            <v-chip
                                v-if="form.last_modified"
                                small
                                dark
                                color="green accent-1"
                                class="green-text"
                            >
                                Terakhir disimpan: {{ (new Date(form.last_modified)).toLocaleString('id-ID') }}
                            </v-chip>
                        </v-col>
                    </v-row>
                    <!-- header end -->
                    <!-- informasi project start -->
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
                                    <label for="">PRK-SKKI</label>
                                    <v-select
                                        :items="skkis.map((skki) => skki.nomor_prk_skki)"
                                        placeholder="Pilih satu atau beberapa PRK-SKKI"
                                        multiple
                                        chips
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        v-model="form.prk_skkis"
                                        @change="save()"
                                    ></v-select>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
                                    class="pt-0"
                                >
                                    <label for="">Nodin</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        v-model="form.nodin"
                                        :hide-details="true"
                                        placeholder="Masukkan nomor Nodin"
                                        @change="save()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
                                    class="pt-0"
                                >
                                    <label for="">Nomor PR</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        v-model="form.nomor_pr"
                                        :hide-details="true"
                                        placeholder="Masukkan nomor PR"
                                        @change="save()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
                                    class="pt-0"
                                >
                                    <label for="">Nama Project</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        v-model="form.nama_project"
                                        :hide-details="true"
                                        placeholder="Masukkan Nama Project"
                                        @change="save()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
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
                                        @change="save()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
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
                                        @change="save()"
                                    ></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="3"
                                    class="pt-0"
                                >
                                    <label for="">Status</label>
                                    <v-select
                                        filled
                                        rounded
                                        dense
                                        v-model="form.status"
                                        :hide-details="true"
                                        placeholder="Pilih tanggal akhir"
                                        :items="['Proses', 'Terkontrak']"
                                        @change="checkStatus()"
                                    ></v-select>
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
                                    <v-select
                                        :items="form.wbs_jasas_option"
                                        placeholder="Pilih PKR"
                                        filled
                                        rounded
                                        :hide-details="true"
                                        dense
                                        v-model="form.wbs_jasas"
                                    ></v-select>
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
                                    <v-select
                                        :items="form.wbs_materials_option"
                                        placeholder="Pilih PKR"
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        v-model="form.wbs_materials"
                                    ></v-select>
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
import DatePicker from './DatePicker.vue'
export default {
    components: { DatePicker },
    props: ['is_show', 'basket', 'skkis'],
    data() {
        return {
            skkis: [],
            form: {
                basket: '',
                last_modified: '',
                prk_skkis: '',
                nodin: '',
                nomor_pr: '',
                nama_project: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                status: '',
                wbs_jasas: '',
                wbs_materials: '',
                wbs_jasas_option: [],
                wbs_materials_option: []
            },
            date_picker: {
                is_show: false,
                tgl: 'awal'
            },
            jasas: [],
            materials: [],
            remove_dialog: {
                is_show: false
            },


            new_pengadaan: {
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
        openDatePicker(tgl) {
            console.log(tgl)
            this.date_picker.tgl = tgl;
            this.date_picker.is_show = true
        },
        saveDate(date) {
            if(this.date_picker.tgl == 'awal') {
                this.form.tanggal_awal = date
            } else {
                this.form.tanggal_akhir = date
            }
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
            this.form.basket = data.basket;
        }
    }
}
</script>

<style>

</style>