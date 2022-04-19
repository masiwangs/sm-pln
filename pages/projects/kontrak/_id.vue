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
                <h4 style="margin-bottom: -.25rem" class="grey--text">Tahap Kontrak</h4>
                <h2>Kontrak #{{ form.nomor_kontrak ? form.nomor_kontrak : '' }}</h2>
            </div>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Informasi Project</div>
                    <div v-if="form.versi_amandemen">
                        <v-chip>Amandemen: {{ form.versi_amandemen }}</v-chip>
                    </div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text class="pt-6">
                    <v-row>
                        <v-col cols="12" md="6" lg="4">
                            <div class="mb-3">
                                <label for="">Nomor Kontrak</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    :disabled="!amandemen.is_amandemen"
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
                                    :disabled="!amandemen.is_amandemen"
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
                                    :disabled="!amandemen.is_amandemen"
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
                                    :disabled="!amandemen.is_amandemen"
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
                                    :disabled="!amandemen.is_amandemen"
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
                                    :disabled="!amandemen.is_amandemen"
                                    :hide-details="true" 
                                    @change="save()"
                                    placeholder="Pilih tanggal akhir" 
                                    type="date"></v-text-field>
                            </div>
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>RAB Jasa</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
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
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>RAB Material</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
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
                <v-btn class="rounded-lg mr-2" elevation="0" dark @click="remove_dialog.is_show = true" color="red">Buang</v-btn>
                <v-dialog v-model="remove_dialog.is_show" max-width="400">
                    <v-card class="rounded-lg">
                        <v-card-title>Buang perubahan?</v-card-title>
                        <v-card-text>
                            <v-container>
                                <p class="mb-4">Data yang dibuang tidak dapat dikembalikan lagi. Lanjutkan?</p>
                                <div class="text-right">
                                    <v-btn light elevation="0" class="rounded-lg mr-2" @click="remove_dialog.is_show = false">Batal</v-btn>
                                    <v-btn dark color="red" elevation="0" class="rounded-lg" @click="remove()">Buang</v-btn>
                                </div>
                            </v-container>
                        </v-card-text>
                    </v-card>
                </v-dialog>
                <v-btn 
                    class="rounded-lg me-2" 
                    elevation="0" 
                    dark 
                    color="yellow darken-2" 
                    @click="amandemen.is_show_dialog = true"
                >Amandemen</v-btn>
                <v-btn class="rounded-lg me-2" elevation="0" dark color="indigo accent-2" @click="done()">Selesai</v-btn>
            </div>
        </v-col>

        <v-dialog
            v-model="amandemen.is_show_dialog"
            max-width="480"
        >
            <v-card>
                <v-card-title>
                    <div>Amandemen</div>
                </v-card-title>
                <v-card-text>
                    <label for="">Nomor Amandemen</label>
                    <v-text-field
                        filled
                        rounded
                        :hide-details="true"
                        dense
                        class="mb-3"
                        v-model="amandemen.data.versi_amandemen"
                    ></v-text-field>
                    <div class="text-right">
                        <v-btn
                            dark
                            color="red"
                            class="rounded-lg"
                            elevation="0"
                            @click="closeAmandemen()"
                        >Batal</v-btn>
                        <v-btn
                            dark
                            color="indigo accent-2"
                            class="rounded-lg"
                            elevation="0"
                            @click="saveAmandemen()"
                        >Amandemen</v-btn>
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
                    text: 'Kontrak',
                    active: true,
                    href: '/projects/kontrak/'
                },
                {
                    text: 'Detail Kontrak',
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
            }
        }
    },
    mounted(){
        this.id = window.location.pathname.split('/')[3];
        this.load();
    },
    methods: {
        load(){
            this.$axios.get('/kontraks/'+this.id)
                .then(res => {
                    this.form = res.data.data;
                    this.jasas = res.data.data.jasas;
                    this.materials = res.data.data.materials;
                })
        },
        done() {
            this.$router.push('/projects/kontrak')
        },
        save(){
            let form_data = new FormData();
            form_data.append('nomor_kontrak', this.form?.nomor_kontrak)
            form_data.append('pelaksana', this.form?.pelaksana)
            form_data.append('direksi_pelaksana', this.form?.direksi_pelaksana)
            form_data.append('tanggal_kontrak', this.form?.tanggal_kontrak)
            form_data.append('tanggal_awal', this.form?.tanggal_awal)
            form_data.append('tanggal_akhir', this.form?.tanggal_akhir)

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
        }
    }
}
</script>

<style>

</style>