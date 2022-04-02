<template>
  <v-row>
    <v-col cols="12" class="mb-4">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div>
        <h4 style="margin-bottom: -.25rem" class="grey--text">Material</h4>
        <h2>List Material</h2>
      </div>
    </v-col>
    <v-col cols="12" class="d-flex flex-row">
        <div class="me-2">
            <v-btn
                dark
                color="indigo accent-2"
                elevation="0"
                class="rounded-lg"
                @click="openNewMaterialDialog()"
            >
                <v-icon v-text="'mdi-plus-box'" class="me-2"></v-icon> Baru
            </v-btn>
            <v-dialog
                v-model="new_material_dialog.is_show"
                max-width="400"
                persistent
            >
                <v-card
                    elevation="0"
                    class="rounded-lg"
                >
                    <v-card-title>Material Baru</v-card-title>
                    <v-card-text>
                        <v-container>
                            <label for="">Kode Normalisasi</label>
                            <v-text-field
                                filled
                                rounded
                                dense
                                :hide-details="true"
                                placeholder="Masukkan kode normalisasi"
                                class="mb-3"
                                v-model="new_material_dialog.data.kode_normalisasi"
                            ></v-text-field>
                            <label for="">Nama Material</label>
                            <v-text-field
                                filled
                                rounded
                                dense
                                :hide-details="true"
                                placeholder="Masukkan nama material"
                                class="mb-3"
                                v-model="new_material_dialog.data.nama_material"
                            ></v-text-field>
                            <label for="">Deskripsi</label>
                            <v-textarea
                                filled
                                rounded
                                dense
                                :hide-details="true"
                                placeholder="Masukkan deskripsi material (optional)"
                                class="mb-3"
                                rows="2"
                                v-model="new_material_dialog.data.deskripsi"
                            ></v-textarea>
                            <label for="">Satuan</label>
                            <v-text-field
                                filled
                                rounded
                                dense
                                :hide-details="true"
                                placeholder="Masukkan satuan"
                                class="mb-3"
                                v-model="new_material_dialog.data.satuan"
                            ></v-text-field>
                            <label for="">Harga</label>
                            <v-text-field
                                filled
                                rounded
                                dense
                                :hide-details="true"
                                placeholder="Masukkan harga (Rp)"
                                prefix="Rp"
                                :suffix="'/'+new_material_dialog.data.satuan"
                                class="mb-4"
                                v-model="new_material_dialog.data.harga"
                            ></v-text-field>
                            <div class="text-right">
                                <v-btn
                                    dark
                                    color="red"
                                    elevation="0"
                                    class="rounded-lg me-2"
                                    @click="cancelNewMaterial()"
                                >Batal</v-btn>
                                <v-btn
                                    dark
                                    color="indigo accent-2"
                                    elevation="0"
                                    class="rounded-lg"
                                    @click="saveNewMaterial()"
                                >Simpan</v-btn>
                            </div>
                        </v-container>
                    </v-card-text>
                </v-card>
            </v-dialog>
        </div>
        <v-btn
            dark
            color="green lighten-1"
            elevation="0"
            class="rounded-lg"
        >
            <v-icon v-text="'mdi-microsoft-excel'" class="me-2"></v-icon> Import .xlsx
        </v-btn>
    </v-col>
    <v-col cols="12">
        <v-card
            elevation="0"
            class="rounded-lg"
        >
            <v-card-text>
                <v-container>
                    <v-data-table
                        :headers="table_header"
                        :items="table_items"
                    >
                        <template
                            v-slot:item.nama_material="{ item }"
                        >
                            <p class="mb-0">{{ item.nama_material }}</p>
                            <small class="grey--text">{{ item.deskripsi }}</small>
                        </template>
                        <template
                            v-slot:item.harga="{ item }"
                        >
                            Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }}/{{ item.satuan }}
                        </template>
                        <template
                            v-slot:item.action="{ item }"
                        >
                            <v-btn
                                dark
                                color="indigo accent-2"
                                icon
                                @click="editMaterial(item)"
                            >
                                <v-icon>mdi-file-document-edit</v-icon>
                            </v-btn>
                            <v-btn
                                dark
                                color="red"
                                icon
                                @click="deleteMaterial(item)"
                            >
                                <v-icon>mdi-delete</v-icon>
                            </v-btn>
                        </template>
                    </v-data-table>
                    <v-dialog
                        v-model="edit_material_dialog.is_show"
                        max-width="400"
                        persistent
                    >
                        <v-card
                            elevation="0"
                            class="rounded-lg"
                        >
                            <v-card-title>Ubah Data Material</v-card-title>
                            <v-card-text>
                                <v-container>
                                    <label for="">Kode Normalisasi</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        placeholder="Masukkan kode normalisasi"
                                        class="mb-3"
                                        v-model="edit_material_dialog.data.kode_normalisasi"
                                    ></v-text-field>
                                    <label for="">Nama Material</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        placeholder="Masukkan nama material"
                                        class="mb-3"
                                        v-model="edit_material_dialog.data.nama_material"
                                    ></v-text-field>
                                    <label for="">Deskripsi</label>
                                    <v-textarea
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        placeholder="Masukkan deskripsi material (optional)"
                                        class="mb-3"
                                        rows="2"
                                        v-model="edit_material_dialog.data.deskripsi"
                                    ></v-textarea>
                                    <label for="">Satuan</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        placeholder="Masukkan satuan"
                                        class="mb-3"
                                        v-model="edit_material_dialog.data.satuan"
                                    ></v-text-field>
                                    <label for="">Harga</label>
                                    <v-text-field
                                        filled
                                        rounded
                                        dense
                                        :hide-details="true"
                                        placeholder="Masukkan harga (Rp)"
                                        prefix="Rp"
                                        :suffix="'/'+edit_material_dialog.data.satuan"
                                        class="mb-4"
                                        v-model="edit_material_dialog.data.harga"
                                    ></v-text-field>
                                    <div class="text-right">
                                        <v-btn
                                            dark
                                            color="red"
                                            elevation="0"
                                            class="rounded-lg me-2"
                                            @click="cancelEditMaterial()"
                                        >Batal</v-btn>
                                        <v-btn
                                            dark
                                            color="indigo accent-2"
                                            elevation="0"
                                            class="rounded-lg"
                                            @click="saveEditMaterial()"
                                        >Simpan</v-btn>
                                    </div>
                                </v-container>
                            </v-card-text>
                        </v-card>
                    </v-dialog>
                    <v-dialog
                        v-model="delete_material_dialog.is_show"
                        max-width="400"
                        persistent
                    >
                        <v-card
                            elevation="0"
                            class="rounded-lg"
                        >
                            <v-card-title>Hapus Material</v-card-title>
                            <v-card-text>
                                <v-container>
                                    <p class="mb-4">Data material yang dihapus tidak dapat dikembalikan lagi. Lanjutkan?</p>
                                    <div class="text-right">
                                        <v-btn
                                            dark
                                            color="red"
                                            elevation="0"
                                            class="rounded-lg me-2"
                                            @click="cancelDeleteMaterial()"
                                        >Batal</v-btn>
                                        <v-btn
                                            dark
                                            color="indigo accent-2"
                                            elevation="0"
                                            class="rounded-lg"
                                            @click="continueDeleteMaterial()"
                                        >Lanjutkan</v-btn>
                                    </div>
                                </v-container>
                            </v-card-text>
                        </v-card>
                    </v-dialog>
                </v-container>
            </v-card-text>
        </v-card>
    </v-col>
  </v-row>
</template>

<script>
    export default {
        data() {
            return {
                breadcrumbItems: [
                    {
                        text: 'Dashboard',
                        active: true,
                        href: '/'
                    },
                    {
                        text: 'Material',
                        active: false,
                    },
                ],
                table_header: [
                    {
                        text: 'Kode Normalisasi',
                        value: 'kode_normalisasi'
                    },
                    {
                        text: 'Nama Material',
                        value: 'nama_material'
                    },
                    {
                        text: 'Satuan',
                        value: 'satuan'
                    },
                    {
                        text: 'Harga',
                        value: 'harga'
                    },
                    {
                        text: 'Action',
                        value: 'action'
                    },
                ],
                table_items: [],
                new_material_dialog: {
                    is_show: false,
                    data: {
                        kode_normalisasi: '',
                        nama_material: '',
                        deskripsi: '',
                        satuan: '',
                        harga: 0
                    }
                },
                edit_material_dialog: {
                    is_show: false,
                    data: {
                        id: '',
                        kode_normalisasi: '',
                        nama_material: '',
                        deskripsi: '',
                        satuan: '',
                        harga: 0
                    }
                },
                delete_material_dialog: {
                    is_show: false,
                    data: {
                        id: '',
                    }
                }
            }
        },
        mounted() {
            this.loadData()
        },
        methods: {
            loadData() {
                this.$axios.get('/materials')
                    .then(res => {
                        this.table_items = res.data.data
                    })
            },
            openNewMaterialDialog() {
                this.new_material_dialog.is_show = true
            },
            cancelNewMaterial(){
                this.new_material_dialog.data.kode_normalisasi = ''
                this.new_material_dialog.data.nama_material = ''
                this.new_material_dialog.data.satuan = ''
                this.new_material_dialog.data.harga = 0
                this.new_material_dialog.is_show = false
            },
            saveNewMaterial() {
                let form_data = new FormData();
                form_data.append('kode_normalisasi', this.new_material_dialog.data.kode_normalisasi);
                form_data.append('nama_material', this.new_material_dialog.data.nama_material);
                form_data.append('deskripsi', this.new_material_dialog.data.deskripsi);
                form_data.append('satuan', this.new_material_dialog.data.satuan);
                form_data.append('harga', this.new_material_dialog.data.harga);
                this.$axios.post('/materials', form_data)
                    .then(res => {
                        this.loadData();
                        this.new_material_dialog.is_show = false
                        this.new_material_dialog.data.kode_normalisasi = ''
                        this.new_material_dialog.data.nama_material = ''
                        this.new_material_dialog.data.deskripsi = ''
                        this.new_material_dialog.data.satuan = ''
                        this.new_material_dialog.data.harga = ''
                    })
            },
            editMaterial(item) {
                this.edit_material_dialog.data.id = item.id
                this.edit_material_dialog.data.kode_normalisasi = item.kode_normalisasi
                this.edit_material_dialog.data.nama_material = item.nama_material
                this.edit_material_dialog.data.satuan = item.satuan
                this.edit_material_dialog.data.deskripsi = item.deskripsi
                this.edit_material_dialog.data.harga = item.harga
                this.edit_material_dialog.is_show = true
            },
            saveEditMaterial() {
                let form_data = new FormData();
                form_data.append('kode_normalisasi', this.edit_material_dialog.data.kode_normalisasi)
                form_data.append('nama_material', this.edit_material_dialog.data.nama_material)
                form_data.append('satuan', this.edit_material_dialog.data.satuan)
                form_data.append('deskripsi', this.edit_material_dialog.data.deskripsi)
                form_data.append('harga', this.edit_material_dialog.data.harga)
                this.$axios.post('/materials/'+this.edit_material_dialog.data.id, form_data)
                    .then(res => {
                        this.loadData();
                        this.edit_material_dialog.is_show = false;
                        this.edit_material_dialog.data.kode_normalisasi = '';
                        this.edit_material_dialog.data.nama_material = '';
                        this.edit_material_dialog.data.satuan = '';
                        this.edit_material_dialog.data.deskripsi = '';
                        this.edit_material_dialog.data.harga = '';
                    })
            },
            cancelEditMaterial() {
                this.edit_material_dialog.is_show = false;
                this.edit_material_dialog.data.kode_normalisasi = '';
                this.edit_material_dialog.data.nama_material = '';
                this.edit_material_dialog.data.satuan = '';
                this.edit_material_dialog.data.deskripsi = '';
                this.edit_material_dialog.data.harga = '';
            },
            deleteMaterial(item) {
                this.delete_material_dialog.data.id = item.id
                this.delete_material_dialog.is_show = true
            },
            cancelDeleteMaterial() {
                this.delete_material_dialog.is_show = false
                this.delete_material_dialog.data.id = ''
            },
            continueDeleteMaterial() {
                this.$axios.delete('/materials/'+this.delete_material_dialog.data.id)
                    .then(res => {
                        this.loadData()
                        this.delete_material_dialog.is_show = false
                        this.delete_material_dialog.data.id = ''
                    })
            }
        }
    }
</script>

<style>

</style>
