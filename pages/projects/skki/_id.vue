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
                <h4 style="margin-bottom: -.25rem" class="grey--text">Tahap SKKI</h4>
                <h2>Detail SKKI</h2>
            </div>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title>
                    <div>Informasi Project</div>
                </v-card-title>
                <v-card-text>
                    <v-row>
                        <v-col
                            cols="12"
                            md="6"
                            lg="4"
                        >
                            <div class="mb-3">
                                <v-chip
                                    v-if="form.last_modified"
                                    small
                                    dark
                                    color="green accent-1"
                                    class="green--text"
                                >Terakhir disimpan: {{ (new Date(form.last_modified)).toLocaleString('id-ID') }}</v-chip>
                            </div>
                            <div class="mb-3">
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
                                    v-model="form.prks"
                                    @change="save()"
                                ></v-select>
                            </div>
                            <div class="mb-3">
                                <label for="">Nomor SKKI</label>
                                <v-text-field 
                                    filled
                                    rounded 
                                    dense 
                                    v-model="form.nomor_skki" 
                                    :hide-details="true"
                                    placeholder="Masukkan nomor PRK"
                                    @change="save()"
                                ></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Nomor PRK-SKKI</label>
                                <v-text-field 
                                    filled 
                                    rounded
                                    dense 
                                    v-model="form.nomor_prk_skki" 
                                    :hide-details="true"
                                    placeholder="Masukkan nomor PRK-SKKI"
                                    @change="save()"
                                ></v-text-field>
                            </div>
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title>
                    <div>RAB Jasa</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <v-row>
                        <v-col
                            cols="12"
                            md="6"
                            lg="4"
                        >
                            <label for="">Nomor WBS Jasa</label>
                            <v-text-field 
                                filled
                                rounded
                                dense 
                                v-model="form.nomor_wbs_jasa" 
                                placeholder="Masukkan nomor WBS jasa"
                                @change="save()"
                                :hide-details="true"
                            ></v-text-field>
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
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title>
                    <div>RAB Material</div>
                </v-card-title>
                <v-divider class="mx-8"></v-divider>
                <v-card-text>
                    <v-row>
                        <v-col
                            cols="12"
                            md="6"
                            lg="4"
                        >
                            <label for="">Nomor WBS Material</label>
                            <v-text-field 
                                filled
                                rounded 
                                dense 
                                v-model="form.nomor_wbs_material" 
                                :hide-details="true"
                                placeholder="Masukkan nomor WBS material"
                                @change="save()"    
                            ></v-text-field>
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
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between align-center">
                    <div>Lampiran Berkas</div>
                    <v-btn text color="success"><v-icon>mdi-upload</v-icon> Upload</v-btn>
                </v-card-title>
                <v-divider class="mx-8"></v-divider>
                <v-card-text>
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
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12">
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
        </v-col>
    </v-row>
</template>

<script>
export default {
    data() {
        return {
            breadcrumb: [
                {
                    text: 'Dashboard',
                    active: true,
                    href: '/'
                },
                {
                    text: 'SKKI',
                    active: true,
                    href: '/projects/skki/'
                },
                {
                    text: 'Detail SKKI',
                    active: false,
                },
            ],
            id: '',
            prks: [],
            form: {},
            jasas: [],
            materials: [],
            remove_dialog: {
                is_show: false
            },
        }
    },
    mounted() {
        this.id = window.location.pathname.split('/')[3];
        this.load();
    },
    methods: {
        load() {
            this.$axios.get('/skkis/'+this.id)
                .then(res => {
                    this.form = res.data.data;
                    this.form.prks = JSON.parse(this.form.prks)
                    this.jasas = res.data.data.jasas;
                    this.materials = res.data.data.materials;
                    console.log(res.data.data)
                })
                .then(() => {
                    this.$axios.get('/prks?basket='+this.form.basket)
                        .then(res => {
                            this.prks = res.data.data
                        })
                })
        },
        done() {
            this.$router.push('/projects/skki')
        },
        save() {
            let form_data = new FormData();
            form_data.append('nomor_skki', this.form.nomor_skki)
            form_data.append('nomor_prk_skki', this.form.nomor_prk_skki)
            form_data.append('nomor_wbs_jasa', this.form.nomor_wbs_jasa)
            form_data.append('nomor_wbs_material', this.form.nomor_wbs_material)
            form_data.append('prks', JSON.stringify(this.form.prks))
            form_data.append('basket', this.form.basket)

            this.$axios.post('/skkis/'+this.id, form_data)
                .then(res => {
                    this.form = res.data.data;
                    this.form.prks = JSON.parse(this.form.prks)
                    this.loadJasa();
                    this.loadMaterial()
                })
        },
        loadJasa() {
            this.$axios.get('/skkis/'+this.id+'/jasas')
                .then(res => {
                    this.jasas = res.data.data
                })
        },
        loadMaterial() {
            this.$axios.get('/skkis/'+this.id+'/materials')
                .then(res => {
                    this.materials = res.data.data
                })
        },
        deleteJasa(id) {
            this.$axios.delete('/skkis/'+this.id+'/jasas/'+id)
                .then(res => {
                    this.loadJasa()
                })
        },
        deleteMaterial(id) {
            this.$axios.delete('/skkis/'+this.id+'/materials/'+id)
                .then(res => {
                    this.loadMaterial()
                })
        },
        remove() {
            this.$axios.delete('/skkis/'+this.id)
                .then(res => {
                    this.$router.push('/projects/skki')
                })
        }
    }
}
</script>

<style>

</style>