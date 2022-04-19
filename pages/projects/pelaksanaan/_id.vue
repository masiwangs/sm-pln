<template>
    <v-row>
        <v-col cols="12">
            <v-breadcrumbs :items="breadcrumb" class="px-0">
                <template v-slot:item="{ item }">
                    <v-breadcrumbs-item
                        :link="true"
                        :exact="true"
                        :to="item.href"
                        :disabled="item.disabled"
                    >
                        {{ item.text }}
                    </v-breadcrumbs-item>
                </template>
            </v-breadcrumbs>
            <div>
                <h4 style="margin-bottom: -.25rem" class="grey--text">Tahap Pelaksanaan</h4>
                <h2>Pelaksanaan #{{ form.nomor_kontrak ? form.nomor_kontrak : '' }}</h2>
            </div>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Informasi Project</div>
                    <div v-if="amandemen.versi_amandemen">
                        <v-chip>{{ amandemen.versi_amandemen }}</v-chip>
                    </div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text class="pt-6">
                    <v-row>
                        <v-col cols="12" md="6" lg="4">
                            <div class="mb-3">
                                <v-chip
                                small
                                dark
                                color="green accent-1"
                                class="green--text"
                            >
                                Terakhir disimpan: {{ (new Date(form.updated_at)).toLocaleString('id-ID') }}
                            </v-chip>
                            </div>
                            <div class="mb-3">
                                <label for="">Nomor Kontrak</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    disabled
                                    v-model="form.nomor_kontrak" 
                                    :hide-details="true"
                                    @change="save()"
                                    placeholder="Masukkan nomor kontrak"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Pelaksana</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    :hide-details="true" 
                                    disabled
                                    v-model="form.pelaksana" 
                                    @change="save()"
                                    placeholder="Masukkan pelaksana"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Direksi</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    :hide-details="true" 
                                    disabled
                                    v-model="form.direksi_pelaksana" 
                                    @change="save()"
                                    placeholder="Masukkan nama direksi"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Tanggal Kontrak</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    :hide-details="true" 
                                    disabled
                                    v-model="form.tanggal_kontrak" 
                                    type="date" 
                                    @change="save()"
                                    placeholder="Pilih tanggal kontrak"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Tanggal Awal</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    v-model="form.tanggal_awal" 
                                    disabled
                                    :hide-details="true" 
                                    @change="save()"
                                    placeholder="Pilih tanggal awal" 
                                    type="date"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Tanggal Akhir</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    v-model="form.tanggal_akhir" 
                                    disabled
                                    :hide-details="true" 
                                    @change="save()"
                                    placeholder="Pilih tanggal akhir" 
                                    type="date"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">SPK</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    v-model="form.spk" 
                                    :hide-details="true" 
                                    @change="save()"
                                    placeholder="Masukkan SPK" 
                                    ></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Progress</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    v-model="form.progress" 
                                    :hide-details="true" 
                                    @change="save()"
                                    placeholder="Progress" 
                                    type="number"
                                    step="0.1"
                                    :suffix="'%'"
                                    ></v-text-field>
                            </div>
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Realisasi Jasa</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <div class="mb-3">
                        <v-chip
                            class="ma-2 me-1"
                        >
                            <strong>Total:</strong> Rp{{ new Intl.NumberFormat('id-ID').format(jasa_section.data.total) }},-
                        </v-chip>
                        <v-chip
                            class="ma-2"
                        >
                            <strong>Saldo:</strong> Rp{{ new Intl.NumberFormat('id-ID').format(jasa_section.data.saldo) }},-
                        </v-chip>
                    </div>
                    <div class="mb-3">
                        <v-tabs>
                            <v-tab @click="jasa_section.view = 'transaction'">Realisasi</v-tab>
                            <v-tab @click="jasa_section.view = 'stock'">RAB</v-tab>
                        </v-tabs>
                    </div>
                    <div>
                        <div v-if="jasa_section.view == 'transaction'">    
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Jasa</th>
                                            <th class="text-left">Nilai</th>
                                            <th class="text-right"></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in jasa_section.data.transaction" :key="item.id">
                                            <td>{{ item.nama_jasa }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-</td>
                                            <td class="text-right"> 
                                                <v-btn
                                                    dark
                                                    color="red"
                                                    elevation="0"
                                                    class="rounded-lg"
                                                    @click="deleteRealisasiJasa(item.id)"
                                                >Hapus</v-btn>
                                            </td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                            <div class="mt-3 text-right">
                                <v-btn
                                    dark
                                    color="indigo accent-2"
                                    elevation="0"
                                    class="rounded-lg"
                                    @click="realisasi_jasa_dialog.is_show = true"
                                >
                                    Realisasi Jasa Baru
                                </v-btn>
                            </div>
                        </div>
                        <div v-else>    
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Jasa</th>
                                            <th class="text-left">Nilai</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in jasa_section.data.stock" :key="item.id">
                                            <td>{{ item.nama_jasa }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-</td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </div>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Realisasi Material</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <div class="mb-3">
                        <v-tabs>
                            <v-tab @click="material_section.view = 'transaction'">Transaksi Material</v-tab>
                            <v-tab @click="material_section.view = 'stock'">Stok Material</v-tab>
                        </v-tabs>
                    </div>
                    <div>
                        <div v-if="material_section.view == 'transaction'">
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">TUG9</th>
                                            <th class="text-left">Normalisasi</th>
                                            <th class="text-left">Nama</th>
                                            <th class="text-left">Jumlah</th>
                                            <th class="text-left">Satuan</th>
                                            <th class="text-left">Harga</th>
                                            <th class="text-left">Transaksi</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in material_section.data.transaction" :key="item.id">
                                            <td>{{ item.tug9 }}</td>
                                            <td>{{ item.kode_normalisasi }}</td>
                                            <td>{{ item.nama_material }}</td>
                                            <td>{{ item.jumlah }}</td>
                                            <td>{{ item.satuan }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-/{{ item.satuan }}</td>
                                            <td>{{ item.transaction == 'in' ? 'Retur' : 'Reservasi' }}</td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                            <div class="mt-3 text-right">
                                <v-btn
                                    dark
                                    color="red"
                                    elevation="0"
                                    class="rounded-lg me-2"
                                    @click="retur_material_dialog.is_show = true"
                                >
                                    Retur
                                </v-btn>
                                <v-btn
                                    dark
                                    color="indigo accent-2"
                                    elevation="0"
                                    class="rounded-lg"
                                    @click="reservasi_material_dialog.is_show = true"
                                >
                                    Reservasi
                                </v-btn>
                            </div>
                        </div>
                        <div v-else>
                            <v-simple-table>
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Normalisasi</th>
                                            <th class="text-left">Nama</th>
                                            <th class="text-left">Jumlah</th>
                                            <th class="text-left">Stok</th>
                                            <th class="text-left">Satuan</th>
                                            <th class="text-left">Harga</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in material_section.data.stock" :key="item.id">
                                            <td>{{ item.kode_normalisasi }}</td>
                                            <td>{{ item.nama_material }}</td>
                                            <td>{{ item.jumlah }}</td>
                                            <td>{{ item.stock }}</td>
                                            <td>{{ item.satuan }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }},-/{{ item.satuan }}</td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </div>
                    </div>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title>
                    <div>Lampiran Berkas</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <v-simple-table>
                        <template v-slot:default>
                            <tbody>
                                <tr>
                                    <td>Berkas_01_02.xlsx</td>
                                    <td class="text-right">
                                        <v-btn text small color="primary">
                                            <v-icon>mdi-download</v-icon> unduh
                                        </v-btn>
                                        <v-btn text small color="red">
                                            <v-icon>mdi-delete</v-icon>
                                        </v-btn>
                                    </td>
                                </tr>
                            </tbody>
                        </template>
                    </v-simple-table>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <div class="text-right">
                <v-btn class="rounded-lg me-2" elevation="0" dark color="indigo accent-2" @click="done()">Selesai</v-btn>
            </div>
        </v-col>

        <v-dialog
            v-model="reservasi_material_dialog.is_show"
            max-width="480"
        >
            <v-card class="rounded-lg">
                <v-card-title>
                    <div><span class="indigo--text accent-2">Reservasi</span> Material Baru</div>
                </v-card-title>
                <v-card-text>
                    <div class="mb-">
                        <label for="">TUG9</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="reservasi_material_dialog.data.tug9"
                            :hide-details="true"
                            dense
                            placeholder="Masukkan kode TUG9"
                        ></v-text-field>
                    </div>
                    <div class="mb-3">
                        <label for="">Kode Normalisasi</label>
                        <v-autocomplete
                            filled
                            rounded
                            :items="material_section.data.stock.map(el => el.kode_normalisasi)"
                            v-model="reservasi_material_dialog.data.kode_normalisasi"
                            :hide-details="true"
                            dense
                            placeholder="Masukkan kode normalisasi material"
                            @change="getDetailMaterial()"
                        ></v-autocomplete>
                    </div>
                    <div class="mb-3">
                        <label for="">Nama Material</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="reservasi_material_dialog.data.nama_material"
                            dense
                            :hide-details="true"
                            placeholder="Masukkan nama material"
                        ></v-text-field>
                    </div>
                    <div class="mb-3">
                        <label for="">Satuan</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="reservasi_material_dialog.data.satuan"
                            dense
                            :hide-details="true"
                            placeholder="Masukkan satuan material"
                        ></v-text-field>
                    </div>
                    <div class="mb-4">
                        <label for="">Qty</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="reservasi_material_dialog.data.qty"
                            dense
                            placeholder="Masukkan jumlah material"
                            :hint="'Stok sisa: '+reservasi_material_dialog.data.stock+' '+reservasi_material_dialog.data.satuan"
                            persistent-hint
                        ></v-text-field>
                    </div>
                    <div class="text-right">
                        <v-btn
                            dark
                            color="red"
                            @click="closeReservasiMaterial()"
                            class="rounded-lg me-2"
                            elevation="0"
                        >
                            Batal
                        </v-btn>
                        <v-btn
                            dark
                            color="indigo accent-2"
                            elevation="0"
                            class="rounded-lg"
                            @click="saveReservasiMaterial()"
                        >
                            {{ reservasi_material_dialog.is_loading ? 'Loading..' : 'Simpan' }}
                        </v-btn>
                    </div>
                </v-card-text>
            </v-card>
        </v-dialog>

        <v-dialog
            v-model="retur_material_dialog.is_show"
            max-width="480"
        >
            <v-card class="rounded-lg">
                <v-card-title>
                    <div><span class="red--text">Retur</span> Material Baru</div>
                </v-card-title>
                <v-card-text>
                    <div class="mb-">
                        <label for="">TUG9</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="retur_material_dialog.data.tug9"
                            :hide-details="true"
                            dense
                            placeholder="Masukkan kode TUG9"
                        ></v-text-field>
                    </div>
                    <div class="mb-3">
                        <label for="">Kode Normalisasi</label>
                        <v-autocomplete
                            filled
                            rounded
                            :items="material_section.data.stock.map(el => el.kode_normalisasi)"
                            v-model="retur_material_dialog.data.kode_normalisasi"
                            :hide-details="true"
                            dense
                            placeholder="Masukkan kode normalisasi material"
                            @change="getDetailReturMaterial()"
                        ></v-autocomplete>
                    </div>
                    <div class="mb-3">
                        <label for="">Nama Material</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="retur_material_dialog.data.nama_material"
                            dense
                            :hide-details="true"
                            placeholder="Masukkan nama material"
                        ></v-text-field>
                    </div>
                    <div class="mb-3">
                        <label for="">Satuan</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="retur_material_dialog.data.satuan"
                            dense
                            :hide-details="true"
                            placeholder="Masukkan satuan material"
                        ></v-text-field>
                    </div>
                    <div class="mb-4">
                        <label for="">Qty</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="retur_material_dialog.data.qty"
                            dense
                            placeholder="Masukkan jumlah material"
                            :hide-details="true"
                        ></v-text-field>
                    </div>
                    <div class="text-right">
                        <v-btn
                            dark
                            color="red"
                            @click="closeReturMaterial()"
                            class="rounded-lg me-2"
                            elevation="0"
                        >
                            Batal
                        </v-btn>
                        <v-btn
                            dark
                            color="indigo accent-2"
                            elevation="0"
                            class="rounded-lg"
                            @click="saveReturMaterial()"
                        >
                            Simpan
                        </v-btn>
                    </div>
                </v-card-text>
            </v-card>
        </v-dialog>

        <v-dialog
            v-model="realisasi_jasa_dialog.is_show"
            max-width="480"
        >
            <v-card class="rounded-lg">
                <v-card-title>
                    <div><span class="indigo--text accent-2">Realisasi</span> Jasa Baru</div>
                </v-card-title>
                <v-card-text>
                    <div class="mb-3">
                        <label for="">Nama Jasa</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="realisasi_jasa_dialog.data.nama_jasa"
                            :hide-details="true"
                            dense
                            placeholder="Masukkan nama jasa"
                        ></v-text-field>
                    </div>
                    <div class="mb-3">
                        <label for="">Nilai Jasa</label>
                        <v-text-field
                            filled
                            rounded
                            v-model="realisasi_jasa_dialog.data.harga"
                            dense
                            :hide-details="true"
                            placeholder="Masukkan nilai jasa"
                        ></v-text-field>
                    </div>
                    <div v-show="realisasi_jasa_dialog.is_error" class="my-4">
                        <v-alert
                            border="left"
                            type="error"
                            elevation="0"
                            dense
                            class="rounded-lg"
                        >
                            {{ realisasi_jasa_dialog.error_message }}
                        </v-alert>
                    </div>
                    <div class="text-right">
                        <v-btn
                            dark
                            color="red"
                            @click="closeRealisasiJasa()"
                            class="rounded-lg me-2"
                            elevation="0"
                        >
                            Batal
                        </v-btn>
                        <v-btn
                            dark
                            color="indigo accent-2"
                            elevation="0"
                            class="rounded-lg"
                            @click="saveRealisasiJasa()"
                        >
                            Simpan
                        </v-btn>
                    </div>
                </v-card-text>
            </v-card>
        </v-dialog>
    </v-row>
</template>

<script>
export default {
    data: () => {
        return {
            breadcrumb: [
                {
                    text: 'Dashboard',
                    active: true,
                    href: '/'
                },
                {
                    text: 'Pelaksanaan',
                    active: true,
                    href: '/projects/pelaksanaan/'
                },
                {
                    text: 'Detail Pelaksanaan',
                    active: false,
                },
            ],
            id: '',
            form: {},
            remove_dialog: {
                is_show: false
            },
            jasas: [],
            materials: [],
            amandemen: {
                is_show_dialog: false,
                is_amandemen: false,
                data: {
                    versi_amandemen: ''
                }
            },
            material_section: {
                view: 'transaction',
                data: {
                    transaction: [],
                    stock: []
                }
            },
            jasa_section: {
                view: 'transaction',
                data: {
                    total: 0,
                    saldo: 0,
                    transaction: [],
                    stock: []
                }
            },
            reservasi_material_dialog: {
                is_show: false,
                is_loading: false,
                data: {
                    tug9: '',
                    kode_normalisasi: '',
                    nama_material: '',
                    satuan: '',
                    harga: '',
                    qty: '',
                    stock: '',
                }
            },
            retur_material_dialog: {
                is_show: false,
                is_loading: false,
                data: {
                    tug9: '',
                    kode_normalisasi: '',
                    nama_material: '',
                    satuan: '',
                    harga: '',
                    qty: '',
                    stock: '',
                }
            },
            realisasi_jasa_dialog: {
                is_show: false,
                is_error: false,
                error_message: '',
                data: {
                    nama_jasa: '',
                    harga: ''
                }
            }
        }
    },
    mounted(){
        this.id = window.location.pathname.split('/')[3];
        this.load();
    },
    methods: {
        load(){
            this.$axios.get('/pelaksanaans/'+this.id)
                .then(res => {
                    this.form = res.data.data;
                    this.jasa_section.data.stock = res.data.data.jasas;
                    this.jasa_section.data.transaction = res.data.data.jasa_transactions;
                    this.material_section.data.stock = res.data.data.materials;
                    this.material_section.data.transaction = res.data.data.material_transactions;

                    let total_jasa = 0;
                    this.jasa_section.data.stock.forEach(el => {
                        total_jasa += el.harga;
                    })

                    let realisasi_jasa = 0;
                    this.jasa_section.data.transaction.forEach(el => {
                        realisasi_jasa += el.harga;
                    })

                    this.jasa_section.data.total = total_jasa;
                    this.jasa_section.data.saldo = total_jasa - realisasi_jasa;
                })  
        },
        done() {
            this.$router.push('/projects/pelaksanaan')
        },
        save(){
            let form_data = new FormData();
            form_data.append('nomor_kontrak', this.form?.nomor_kontrak)
            form_data.append('pelaksana', this.form?.pelaksana)
            form_data.append('direksi_pelaksana', this.form?.direksi_pelaksana)
            form_data.append('tanggal_kontrak', this.form?.tanggal_kontrak)
            form_data.append('tanggal_awal', this.form?.tanggal_awal)
            form_data.append('tanggal_akhir', this.form?.tanggal_akhir)
            form_data.append('progress', this.form?.progress)
            form_data.append('spk', this.form?.spk)

            this.$axios.post('/kontraks/'+this.id, form_data)
                .then(res => {
                    console.log('success')
                })
        },
        closeAmandemen() {
            this.amandemen = {
                is_show_dialog: false,
                is_amandemen: false,
                data: {}
            }
        },
        saveAmandemen() {
            let form_data = new FormData();
            form_data.append('versi_amandemen', this.amandemen.data.versi_amandemen)
            this.$axios.post('/kontraks/'+this.id, form_data)
                .then(res => {
                    this.amandemen.is_amandemen = true
                    this.amandemen.is_show_dialog = false;
                })
        },
        getDetailMaterial() {
            let found = this.material_section.data.stock.filter(el => el.kode_normalisasi == this.reservasi_material_dialog.data.kode_normalisasi)
            let stock = 0;
            found.forEach(el => {
                stock += el.stock;
            })
            this.reservasi_material_dialog.data = found[0];
            this.reservasi_material_dialog.data.stock = stock;
        },
        getDetailReturMaterial() {
            let found = this.material_section.data.stock.filter(el => el.kode_normalisasi == this.retur_material_dialog.data.kode_normalisasi)
            let stock = 0;
            found.forEach(el => {
                stock += el.stock;
            })
            this.retur_material_dialog.data = found[0];
            this.retur_material_dialog.data.stock = stock;
        },
        closeReservasiMaterial() {
            this.reservasi_material_dialog.is_show = false;
            this.reservasi_material_dialog.data = {
                tug9: '',
                kode_normalisasi: '',
                nama_material: '',
                satuan: '',
                harga: '',
                qty: '',
                stock: '',
            };
        },
        closeReturMaterial() {
            this.retur_material_dialog.is_show = false;
            this.retur_material_dialog.data = {
                tug9: '',
                kode_normalisasi: '',
                nama_material: '',
                satuan: '',
                harga: '',
                qty: '',
                stock: '',
            };
            console.log(this.reservasi_material_dialog)
        },
        saveReservasiMaterial() {
            let form_data = new FormData();
            form_data.append('tug9', this.reservasi_material_dialog.data.tug9);
            form_data.append('kode_normalisasi', this.reservasi_material_dialog.data.kode_normalisasi);
            form_data.append('nama_material', this.reservasi_material_dialog.data.nama_material);
            form_data.append('satuan', this.reservasi_material_dialog.data.satuan);
            form_data.append('harga', this.reservasi_material_dialog.data.harga);
            form_data.append('jumlah', this.reservasi_material_dialog.data.qty);
            form_data.append('transaction', 'out');
            form_data.append('kontrak_id', this.id);

            this.$axios.post('/pelaksanaans/'+this.id+'/transaction/material', form_data)
                .then(res => {
                    if(res.status > 200) {
                        console.log(res)
                    } else {
                        this.reservasi_material_dialog.is_show = false;
                        this.load();
                        this.reservasi_material_dialog.data = {
                            tug9: '',
                            kode_normalisasi: '',
                            nama_material: '',
                            satuan: '',
                            harga: '',
                            qty: '',
                            stock: '',
                        };
                    }
                })
                .catch(e => {
                    console.log(e)
                })
        },
        saveReturMaterial() {
            let form_data = new FormData();
            form_data.append('tug9', this.retur_material_dialog.data.tug9);
            form_data.append('kode_normalisasi', this.retur_material_dialog.data.kode_normalisasi);
            form_data.append('nama_material', this.retur_material_dialog.data.nama_material);
            form_data.append('satuan', this.retur_material_dialog.data.satuan);
            form_data.append('harga', this.retur_material_dialog.data.harga);
            form_data.append('jumlah', this.retur_material_dialog.data.qty);
            form_data.append('transaction', 'in');
            form_data.append('kontrak_id', this.id);

            this.$axios.post('/pelaksanaans/'+this.id+'/transaction/material', form_data)
                .then(res => {
                    if(res.status > 200) {
                        console.log(res)
                    } else {
                        this.load();
                        this.retur_material_dialog.is_show = false;
                        this.retur_material_dialog.data = {
                            tug9: '',
                            kode_normalisasi: '',
                            nama_material: '',
                            satuan: '',
                            harga: '',
                            qty: '',
                            stock: '',
                        };
                    }
                })
                .catch(e => {
                    console.log(e)
                    this.load();
                    this.retur_material_dialog.data = {
                        tug9: '',
                        kode_normalisasi: '',
                        nama_material: '',
                        satuan: '',
                        harga: '',
                        qty: '',
                        stock: '',
                    };
                })
        },
        saveRealisasiJasa() {
            let form_data = new FormData();
            form_data.append('nama_jasa', this.realisasi_jasa_dialog.data.nama_jasa);
            form_data.append('harga', this.realisasi_jasa_dialog.data.harga);
            form_data.append('kontrak_id', this.id);

            this.$axios.post('/pelaksanaans/'+this.id+'/transaction/jasa', form_data)
                .then(res => {
                    this.load();
                    this.realisasi_jasa_dialog = {
                        is_show: false,
                        is_error: false,
                        error_message: '',
                        data: {
                            nama_jasa: '',
                            harga: ''
                        }
                    }
                })
                .catch(e => {
                    this.realisasi_jasa_dialog.error_message = e.response.status +': '+e.response.data.message;
                    this.realisasi_jasa_dialog.is_error = true;
                })
        },
        closeRealisasiJasa(){
            this.realisasi_jasa_dialog = {
                is_show: false,
                is_error: false,
                error_message: '',
                data: {
                    nama_jasa: '',
                    harga: ''
                }
            }
        },
        deleteRealisasiJasa(id) {
            this.$axios.delete('/pelaksanaans/'+this.id+'/transaction/jasa/'+id)
                .then(res => {
                    this.load();
                })
        }
    },
}
</script>

<style>

</style>