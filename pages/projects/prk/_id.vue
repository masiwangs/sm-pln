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
                <h4 style="margin-bottom: -.25rem" class="grey--text">Tahap PRK</h4>
                <h2>Detail PRK</h2>
            </div>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title>
                    <div>Informasi Project</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <v-row>
                        <v-col
                            cols="12"
                            md="6"
                            lg="4"
                        >
                            <div class="mb-3">
                                <v-chip 
                                    v-if="project.updated_at"
                                    small
                                    dark
                                    color="green accent-1"
                                    class="green--text"
                                >Terakhir disimpan: {{ (new Date(project.updated_at)).toLocaleString('id-ID') }}</v-chip>
                            </div>
                            <div class="mb-3">
                                <label for="">Nama Project</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    :hide-details="true" 
                                    dense 
                                    v-model="project.nama_project" 
                                    @change="save()"
                                    placeholder="Masukkan nama project"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Nomor PRK</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    :hide-details="true" 
                                    dense 
                                    v-model="project.nomor_prk" 
                                    @change="save()"
                                    placeholder="Masukkan nomor PRK"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Lot Number</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    :hide-details="true" 
                                    dense 
                                    v-model="project.lot_number" 
                                    @change="save()"
                                    placeholder="Masukkan lot number"></v-text-field>
                            </div>
                            <div class="mb-3">
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
                                    <h4 class="mr-3">( {{ project.prioritas }} )</h4>( <span role="button" class="primary--text" @click="prioritas = 0"><u>reset</u></span> )
                                    </div>
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
                        <v-col cols="12">
                            <v-btn 
                                class="rounded-lg me-2" 
                                elevation="0" 
                                color="indigo accent-2"
                                @click="new_jasa_dialog.is_show = true" dark 
                            >
                                <v-icon class="mr-2">mdi-plus-box</v-icon> Baru
                            </v-btn>
                            <v-btn 
                                dark 
                                class="rounded-lg" 
                                elevation="0" 
                                color="green dark-3"
                                @click="import_jasa_dialog.is_show = true"
                            >
                                <v-icon class="mr-2">mdi-microsoft-excel</v-icon> Import .xlsx
                            </v-btn>
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
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <v-row>
                        <v-col cols="12">
                            <v-btn 
                                @click="new_material_dialog.is_show = true" 
                                dark 
                                class="rounded-lg" 
                                elevation="0" 
                                color="indigo accent-2"
                            >
                                <v-icon class="mr-2">mdi-plus-box</v-icon> Baru
                            </v-btn>
                            <v-btn 
                                dark 
                                class="rounded-lg" 
                                elevation="0" 
                                color="green dark-3"
                                @click="import_material_dialog.is_show = true"
                            >
                                <v-icon class="mr-2">mdi-microsoft-excel</v-icon> Import .xlsx
                            </v-btn>
                        </v-col>
                        <v-col cols="12">
                            <v-simple-table v-if="materials">
                                <template v-slot:default>
                                    <thead>
                                        <tr>
                                            <th class="text-left">Kode Normalisasi</th>
                                            <th class="text-left">Nama Material</th>
                                            <th class="text-left">Jumlah</th>
                                            <th class="text-left">Satuan</th>
                                            <th class="text-left">Harga</th>
                                            <th class="text-left">Total</th>
                                            <th class="text-left" width="10%">Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(item, index) in materials" :key="index">
                                            <td>{{ item.kode_normalisasi }}</td>
                                            <td>{{ item.nama_material }}</td>
                                            <td>{{ item.jumlah }}</td>
                                            <td>{{ item.satuan }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }}/{{ item.satuan }}</td>
                                            <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga * item.jumlah) }}</td>
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
                
                <v-btn 
                    class="rounded-lg me-2" 
                    elevation="0" 
                    dark 
                    color="indigo accent-2"
                    @click="done()"
                >Selesai</v-btn>
            </div>
        </v-col>

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
                                v-model="new_jasa_dialog.data.nama_jasa"
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
                                v-model="new_jasa_dialog.data.harga"
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

        <v-dialog
            v-model="import_jasa_dialog.is_show"
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
                                    @click="import_jasa_dialog.is_show = false"
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
                            <v-autocomplete
                                filled
                                rounded
                                :hide-details="true"
                                :items="new_material_dialog.materials.map(el => el.kode_normalisasi)"
                                dense
                                class="mb-3"
                                placeholder="Masukkan kode normalisasi"
                                v-model="new_material_dialog.data.kode_normalisasi"
                                @change="getDetailMaterial()"
                            ></v-autocomplete>
                            <label for="">Nama Material</label>
                            <v-text-field
                                filled
                                rounded
                                :hide-details="true"
                                dense
                                class="mb-3"
                                placeholder="Masukkan nama material"
                                v-model="new_material_dialog.data.nama_material"
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
                            <label for="">Harga</label>
                            <v-text-field
                                filled
                                rounded
                                :hide-details="true"
                                dense
                                type="number"
                                class="mb-3"
                                placeholder="Masukkan harga material"
                                :suffix="'/'+new_material_dialog.data.satuan"
                                v-model="new_material_dialog.data.harga"
                            ></v-text-field>
                            <label for="">Jumlah</label>
                            <v-text-field
                                filled
                                rounded
                                :hide-details="true"
                                dense
                                class="mb-4"
                                placeholder="Masukkan jumlah material"
                                v-model="new_material_dialog.data.jumlah"
                                :suffix="new_material_dialog.data.satuan"
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

        <v-dialog
            v-model="import_material_dialog.is_show"
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
                                    @click="import_material_dialog.is_show = false"
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
                    text: 'PRK',
                    active: true,
                    href: '/projects/prk/'
                },
                {
                    text: 'Detail PRK',
                    active: false,
                },
            ],
            id: '',
            project: {
                nama_project: '',
                nomor_prk: '',
                lot_number: '',
                prioritas: 0,
            },
            prioritas: 0,
            new_jasa_dialog: {
                is_show: false,
                data: {
                    nama_jasa: '',
                    harga: '',
                }
            },
            import_jasa_dialog: {
                is_show: false,
            },
            jasas: [],
            new_material_dialog: {
                is_show: false,
                materials: [],
                data: {
                    kode_normalisasi: '',
                    nama_material: '',
                    satuan: '',
                    harga: '',
                    jumlah: ''
                }
            },
            import_material_dialog: {
                is_show: false,
            },
            materials: [],
            remove_dialog: {
                is_show: false,
            },
        }
    },
    mounted() {
        this.id = window.location.pathname.split('/')[3];
        this.load();
    },
    methods: {
        load() {
            this.$axios.get('/prks/'+this.id)
                .then(res => {
                    this.project = res.data.data;
                    this.prioritas = res.data.data.prioritas;
                    this.jasas = res.data.data.jasas;
                    this.materials = res.data.data.materials;
                })
                .then(() => {
                    this.$axios.get('/materials')
                        .then(res => {
                            this.new_material_dialog.materials = res.data.data
                        })
                })
        },
        save() {
            let form_data = new FormData();
            form_data.append('nama_project', this.project.nama_project);
            form_data.append('nomor_prk', this.project.nomor_prk);
            form_data.append('lot_number', this.project.lot_number);
            form_data.append('prioritas', this.project.prioritas);

            this.$axios.post('/prks/'+this.id, form_data)
                .then(res => {
                    this.project = res.data.data;
                    this.prioritas = res.data.data.prioritas;
                })
        },
        done() {
            this.$router.push('/projects/prk')
        },
        remove() {
            this.$axios.delete('/prks/'+this.id)
                .then(res => {
                    this.$router.push('/projects/prk')
                })
        },
        cancelNewJasa() {
            this.new_jasa_dialog.is_show = false;
        },
        saveNewJasa() {
            let form_data = new FormData();
            form_data.append('nama_jasa', this.new_jasa_dialog.data.nama_jasa);
            form_data.append('harga', this.new_jasa_dialog.data.harga);
            this.$axios.post('/prks/'+this.id+'/jasas', form_data)
                .then(res => {
                    this.loadJasa();
                    this.new_jasa_dialog.data = {
                        nama_jasa: '',
                        harga: '',
                    }
                    this.new_jasa_dialog.is_show = false;
                })
        },
        deleteJasa() {
            this.$axios.delete('/prks/'+this.id+'/jasas/'+id)
                .then(res => {
                    this.loadJasa()
                })
        },
        deleteMaterial(id) {
            this.$axios.delete('/prks/'+this.id+'/materials/'+id)
                .then(res => {
                    this.loadMaterial()
                })
        },
        cancelNewMaterial() {
            this.new_material_dialog.is_show = false;
        },
        saveNewMaterial() {
            let form_data = new FormData();
            form_data.append('kode_normalisasi', this.new_material_dialog.data.kode_normalisasi)
            form_data.append('nama_material', this.new_material_dialog.data.nama_material)
            form_data.append('jumlah', this.new_material_dialog.data.jumlah)
            form_data.append('harga', this.new_material_dialog.data.harga)
            form_data.append('satuan', this.new_material_dialog.data.satuan)
            this.$axios.post('/prks/'+this.id+'/materials', form_data)
                .then(res => {
                    this.loadMaterial();
                    this.new_material_dialog.data = {
                        kode_normalisasi: '',
                        nama_material: '',
                        satuan: '',
                        harga: '',
                        jumlah: ''
                    }
                    this.new_material_dialog.is_show = false;
                })
        },
        loadJasa() {
            this.$axios.get('/prks/'+this.id+'/jasas')
                .then(res => {
                    this.jasas = res.data.data
                })
        },
        loadMaterial(){
            this.$axios.get('/prks/'+this.id+'/materials')
                .then(res => {
                    this.materials = res.data.data
                })
        },
        getDetailMaterial() {
            let autocomplete_items = JSON.parse(JSON.stringify(this.new_material_dialog.materials));
            let material = autocomplete_items.filter(el => el.kode_normalisasi == this.new_material_dialog.data.kode_normalisasi)
            this.new_material_dialog.data.nama_material = material[0].nama_material
            this.new_material_dialog.data.satuan = material[0].satuan
            this.new_material_dialog.data.harga = material[0].harga
        }
    },
    watch: {
        prioritas(val) {
            this.project.prioritas = val
            console.log(val)
            this.save()
        },
    }
}
</script>

<style>

</style>