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
                                class="green--text"
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
                                        v-model="form.nomor_prk_skkis"
                                        @change="save()"
                                    ></v-select>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col
                                    cols="12"
                                    md="6"
                                    lg="4"
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
                                    lg="4"
                                    class="pt-0"
                                >
                                    <label for="">Tanggal Nodin</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        v-model="form.tanggal_nodin"
                                        :hide-details="true"
                                        placeholder="Pilih tanggal nodin"
                                        type="date"
                                        @change="save()"
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
                                    lg="4"
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
                                    lg="4"
                                    class="pt-0"
                                >
                                    <label for="">Status</label>
                                    <v-select
                                        filled
                                        rounded
                                        dense
                                        v-model="form.status"
                                        :hide-details="true"
                                        placeholder="Pilih status pengadaan"
                                        :items="['proses', 'terkontrak']"
                                        @change="changeStatus()"
                                    ></v-select>
                                </v-col>
                            </v-row>
                            <v-card 
                                class="mt-4"
                                v-show="kontrak.is_show"
                            >
                                <v-card-title>Detail kontrak</v-card-title>
                                <v-card-text>
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
                                                :hide-details="true"
                                                placeholder="Masukkan nomor kontrak"
                                                v-model="kontrak.nomor_kontrak"
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
                                            <label for="">Tanggal Kontrak</label>
                                            <v-text-field
                                                filled
                                                rounded
                                                dense
                                                :hide-details="true"
                                                v-model="kontrak.tanggal_kontrak"
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
                                                v-model="kontrak.tanggal_awal"
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
                                                v-model="kontrak.tanggal_akhir"
                                                :hide-details="true"
                                                placeholder="Pilih tanggal akhir"
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
                                            <label for="">Pelaksana</label>
                                            <v-text-field
                                                filled
                                                rounded
                                                dense
                                                v-model="kontrak.pelaksana"
                                                :hide-details="true"
                                                placeholder="Masukkan pelaksana"
                                            ></v-text-field>
                                        </v-col>
                                    </v-row>
                                    <v-row class="mb-4">
                                        <v-col
                                            cols="12"
                                            md="6"
                                            lg="4"
                                            class="pt-0"
                                        >
                                            <label for="">Direksi Pelaksana</label>
                                            <v-text-field
                                                filled
                                                rounded
                                                dense
                                                v-model="kontrak.direksi_pelaksana"
                                                :hide-details="true"
                                                placeholder="Masukkan direksi pelaksana"
                                            ></v-text-field>
                                        </v-col>
                                    </v-row>
                                    <div>
                                        <v-alert
                                            v-if="!kontrak.success"
                                            dense
                                            type="info"
                                            class="mt-4"
                                            border="left"
                                            colored-border
                                        >
                                            <small>
                                                Lengkapi informasi di atas untuk mengubah <strong>status pengadaan</strong> mejadi <strong><q>Terkontrak</q></strong>.
                                            </small>
                                        </v-alert>
                                        <v-alert
                                            v-if="kontrak.success"
                                            dense
                                            type="success"
                                            class="mt-4"
                                            border="left"
                                            colored-border
                                        >
                                            <small>
                                                Sukses! Tersimpan di tahap <strong><q>Terkontrak</q></strong>.
                                            </small>
                                        </v-alert>
                                    </div>
                                    <div 
                                        v-if="!kontrak.success"
                                        class="text-right mt-4"
                                    >
                                        <v-btn
                                            elevation="0"
                                            class="rounded-lg mr-2"
                                            dark
                                            color="red"
                                            @click="hideKontrak()"
                                        >Batal</v-btn>
                                        <v-btn
                                            elevation="0"
                                            class="rounded-lg"
                                            dark
                                            color="indigo accent-2"
                                            @click="saveKontrak()"
                                        >Simpan</v-btn>
                                    </div>
                                </v-card-text>
                            </v-card>
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
                                <v-col cols="12" md="6" lg="4" class="pb-0">
                                    <label for="">Nomor WBS Jasa</label>
                                    <v-select
                                        :items="form.wbs_jasas_option"
                                        placeholder="Pilih PKR"
                                        filled
                                        rounded
                                        multiple
                                        :hide-details="true"
                                        dense
                                        @change="save()"
                                        v-model="form.nomor_wbs_jasas"
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
                            <h2>RAB Material</h2>
                        </v-col>
                        <v-col cols="12">
                            <v-row>
                                <v-col cols="12" md="6" lg="4" class="pb-0">
                                    <label for="">Nomor WBS Material</label>
                                    <v-select
                                        :items="form.wbs_materials_option"
                                        placeholder="Pilih PKR"
                                        filled
                                        rounded
                                        dense
                                        multiple
                                        :hide-details="true"
                                        @change="save()"
                                        v-model="form.nomor_wbs_materials"
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
</template>

<script>
import DatePicker from './DatePicker.vue'
export default {
    components: { DatePicker },
    props: ['is_show', 'basket', 'skkis', 'data'],
    data() {
        return {
            form: {
                id: '',
                basket: '',
                last_modified: '',
                nomor_prk_skkis: [],
                nodin: '',
                tanggal_nodin: '',
                nomor_pr: '',
                nama_project: '',
                status: '',
                nomor_wbs_jasas: [],
                nomor_wbs_materials: [],
                wbs_jasas_option: [],
                wbs_materials_option: [],
            },
            date_picker: {
                is_show: false,
                tgl: 'awal'
            },
            jasas: [],
            materials: [],
            kontrak: {
                is_show: false,
                nomor_kontrak: '',
                tanggal_kontrak: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                pelaksana: '',
                direksi_pekerjaan: '',
                success: false
            },

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
            dummy_prk: [
                'PRK1', 'PRK2', 'PRK3', 'PRK4', 'PRK5', 'PRK6', 'PRK7', 'PRK8', 'PRK9',
            ]
        }
    },
    methods: {
        done() {
            this.form = {
                id: '',
                basket: '',
                last_modified: '',
                nomor_prk_skkis: [],
                nodin: '',
                tanggal_nodin: '',
                nomor_pr: '',
                nama_project: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                status: '',
                nomor_wbs_jasas: [],
                nomor_wbs_materials: [],
                wbs_jasas_option: [],
                wbs_materials_option: []
            }
            this.kontrak = {
                is_show: false,
                nomor_kontrak: '',
                tanggal_kontrak: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                pelaksana: '',
                direksi_pelaksana: '',
                success: false
            }
            this.$emit('reload_data')
            this.$emit('hide_dialog')
        },
        save(with_status = false){
            // clean array
            let nomor_prk_skkis = this.form.nomor_prk_skkis.filter(el => { if(el.length) { return el }})
            let nomor_wbs_jasas = this.form.nomor_wbs_jasas.filter(el => { if(el.length) { return el } })
            let nomor_wbs_materials = this.form.nomor_wbs_materials.filter(el => { if(el.length) { return el } })
            
            let form_data = new FormData();
            form_data.append('nomor_prk_skkis', JSON.stringify(nomor_prk_skkis));
            form_data.append('nodin', this.form.nodin);
            form_data.append('tanggal_nodin', this.form.tanggal_nodin);
            form_data.append('nomor_pr', this.form.nomor_pr);
            form_data.append('nama_project', this.form.nama_project);
            form_data.append('nomor_wbs_jasas', JSON.stringify(nomor_wbs_jasas));
            form_data.append('nomor_wbs_materials', JSON.stringify(nomor_wbs_materials));
            form_data.append('basket', this.form.basket);

            if(with_status) {
                form_data.append('status', this.form.status);
                if(this.form.status == 'proses') {
                    this.kontrak = {
                        is_show: false,
                        nomor_kontrak: '',
                        tanggal_kontrak: '',
                        tanggal_awal: '',
                        tanggal_akhir: '',
                        pelaksana: '',
                        direksi_pelaksana: '',
                        success: false
                    }
                }
            }

            let is_filled = this.form.nomor_prk_skkis.length || this.form.nodin || this.form.tanggal_nodin || this.form.nomor_pr || this.form.nama_project || this.form.tanggal_awal || this.form.tanggal_akhir || this.form.nomor_wbs_jasas.length || this.form.nomor_wbs_materials.length;

            if(this.form.id && is_filled) {
                this.$axios.post('/pengadaans/'+this.form.id, form_data)
                    .then(res => {
                        console.log(res.data.data)
                        this.form.last_modified = res.data.data.updated_at;
                        this.loadJasa();
                        this.loadMaterial();
                        this.loadWbsOption();
                    })
            } else {
                this.$axios.post('/pengadaans', form_data)
                    .then(res => {
                        console.log(res.data.data)
                        this.form.id = res.data.data.id;
                        this.form.last_modified = res.data.data.updated_at;
                        this.form.nomor_wbs_jasas = JSON.parse(res.data.data.nomor_wbs_jasas)
                        this.form.nomor_wbs_materials = JSON.parse(res.data.data.nomor_wbs_materials)
                        this.loadJasa();
                        this.loadMaterial();
                        this.loadWbsOption();
                    })
            }
        },
        changeStatus() {
            if(this.form.status.toLowerCase() == 'terkontrak') {
                this.kontrak.is_show = true;
            } else {
                this.save(true)
            }
        },
        hideKontrak() {
            this.kontrak = {
                is_show: false,
                nomor_kontrak: '',
                tanggal_kontrak: '',
                tanggal_awal: '',
                tanggal_akhir: '',
                pelaksana: '',
                direksi_pelaksana: '',
                success: false
            }
            this.form.status = '';
        },
        saveKontrak() {
            let form_data = new FormData();
            form_data.append('nomor_kontrak', this.kontrak.nomor_kontrak);
            form_data.append('tanggal_kontrak', this.kontrak.tanggal_kontrak);
            form_data.append('tanggal_awal', this.kontrak.tanggal_awal);
            form_data.append('tanggal_akhir', this.kontrak.tanggal_akhir);
            form_data.append('pelaksana', this.kontrak.pelaksana);
            form_data.append('direksi_pelaksana', this.kontrak.direksi_pelaksana);
            form_data.append('pengadaan_id', this.form.id);
            this.$axios.post('/kontraks', form_data)
                .then(res => {
                    if(res.status == 201) {
                        this.kontrak.success = true;
                    }
                })
        },
        loadJasa() {
            this.$axios.get('/pengadaans/'+this.form.id+'/jasas')
                .then(res => {
                    this.jasas = res.data.data
                    console.log(this.jasas)
                })
        },
        loadMaterial() {
            this.$axios.get('/pengadaans/'+this.form.id+'/materials')
                .then(res => {
                    this.materials = res.data.data
                    console.log(this.materials)
                })
        },
        loadWbsOption() {
            this.$axios.get('/pengadaans/'+this.form.id+'/prk_skkis')
                .then(res => {
                    this.form.wbs_jasas_option = res.data.data.map((skki) => skki.nomor_wbs_jasa)
                    this.form.wbs_materials_option = res.data.data.map((skki) => skki.nomor_wbs_material)
                })
        },
        remove() {
            this.$axios.delete('/pengadaans/'+this.form.id)
                .then(res => {
                    this.form = {
                        id: '',
                        basket: '',
                        last_modified: '',
                        nomor_prk_skkis: [],
                        nodin: '',
                        tanggal_nodin: '',
                        nomor_pr: '',
                        nama_project: '',
                        tanggal_awal: '',
                        tanggal_akhir: '',
                        status: '',
                        nomor_wbs_jasas: [],
                        nomor_wbs_materials: [],
                        wbs_jasas_option: [],
                        wbs_materials_option: []
                    }
                    this.kontrak = {
                        is_show: false,
                        nomor_kontrak: '',
                        tanggal_kontrak: '',
                        tanggal_awal: '',
                        tanggal_akhir: '',
                        pelaksana: '',
                        direksi_pekerjaan: ''
                    }
                    this.$emit('reload_data')
                    this.$emit('hide_dialog')
                })
                
        }
    },
    watch: {
        data(data) {
            this.form.basket = data.basket;
            this.form.id = data.id;
            this.form.basket = data.basket;
            this.form.last_modified = data.updated_at;
            this.form.nomor_prk_skkis = data.nomor_prk_skkis ? JSON.parse(data.nomor_prk_skkis) : [];
            this.form.nodin = data.nodin;
            this.form.tanggal_nodin = data.tanggal_nodin;
            this.form.nomor_pr = data.nomor_pr;
            this.form.nama_project = data.nama_project;

            this.form.status = data.status;
            this.form.nomor_wbs_jasas = data.nomor_wbs_jasas ? JSON.parse(data.nomor_wbs_jasas) : [];
            this.form.nomor_wbs_materials = data.nomor_wbs_materials ? JSON.parse(data.nomor_wbs_materials) : [];
            this.jasas = data.jasas;
            this.materials = data.materials;

            console.log('opened form', data)

            this.loadWbsOption()
        }
    }
}
</script>

<style>

</style>