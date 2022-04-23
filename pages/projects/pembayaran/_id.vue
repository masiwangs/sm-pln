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
                <h4 style="margin-bottom: -.25rem" class="grey--text">Tahap Pembayaran</h4>
                <h2>Pembayaran #{{ form.nomor_kontrak ? form.nomor_kontrak : '' }}</h2>
            </div>
        </v-col>
        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Informasi Project</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text class="pt-6">
                    <v-row>
                        <v-col cols="12" md="6" lg="4">
                            <div class="mb-3">
                                <label for="">SPK</label>
                                <v-text-field 
                                    filled 
                                    rounded 
                                    dense 
                                    disabled
                                    v-model="form.spk" 
                                    :hide-details="true"
                                    @change="save()"
                                    placeholder="Masukkan SPK"></v-text-field>
                            </div>
                            <div class="mb-3">
                                <label for="">Status</label>
                                <v-select
                                    filled 
                                    rounded 
                                    dense 
                                    :hide-details="true" 
                                    :items="[
                                        'DALAM PELAKSANAAN', 
                                        'ADMINISTRASI PROYEK',
                                        'OUTSTANDING',
                                        'SELESAI BAYAR'
                                    ]"
                                    v-model="form.status" 
                                    @change="save()"
                                    placeholder="Pilih status"></v-select>
                            </div>
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-card>
        </v-col>

        <v-col cols="12">
            <v-card elevation="0" class="rounded-lg">
                <v-card-title class="d-flex justify-space-between">
                    <div>Tagihan Jasa</div>
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
                                <tr v-for="item in form.jasa_transactions" :key="item.id">
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
                    <div>Tagihan Material</div>
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
                                <tr v-for="item in form.material_transactions" :key="item.id">
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
                <v-card-title class="d-flex justify-space-between">
                    <div>Pembayaran</div>
                </v-card-title>
                <v-divider class="mx-4"></v-divider>
                <v-card-text>
                    <v-simple-table>
                        <template v-slot:default>
                            <thead>
                                <tr>
                                    <th class="text-left">Nilai</th>
                                    <th class="text-left">Keterangan</th>
                                    <th class="text-left"></th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="item in form.pembayarans" :key="item.id">
                                    <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.nominal) }}</td>
                                    <td>{{ item.keterangan }}</td>
                                    <td class="text-right">
                                        <v-btn
                                            dark
                                            color="red"
                                            elevation="0"
                                            class="rounded-lg"
                                        >Hapus</v-btn>
                                    </td>
                                </tr>
                            </tbody>
                        </template>
                    </v-simple-table>
                    <div class="text-right mt-3">
                        <v-btn
                            dark
                            color="indigo accent-2"
                            elevation="0"
                            class="rounded-lg"
                            @click="pembayaran_dialog.is_show = true"
                        >Pembayaran Baru</v-btn>
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
                <v-btn class="rounded-lg me-2" elevation="0" dark color="indigo accent-2" @click="done()">Selesai</v-btn>
            </div>
        </v-col>

        <v-dialog 
            v-model="pembayaran_dialog.is_show" 
            max-width="400"
        >
            <v-card class="rounded-lg">
                <v-card-title>Pembayaran Baru</v-card-title>
                <v-card-text>
                    <div class="mb-3">
                        <label for="">Nilai</label>
                        <v-text-field
                            filled
                            rounded
                            :hide-details="true"
                            dense
                            v-model="pembayaran_dialog.data.nilai"
                            prefix="Rp"
                        ></v-text-field>
                    </div>
                    <div class="mb-4">
                        <label for="">Keterangan</label>
                        <v-textarea
                            filled
                            rounded
                            :hide-details="true"
                            dense
                            v-model="pembayaran_dialog.data.keterangan"
                        ></v-textarea>
                    </div>
                    <div class="text-right">
                        <v-btn
                            dark
                            color="red"
                            elevation="0"
                            class="rounded-lg me-2"
                            @click="closePembayaranDialog()"
                        >Batal</v-btn>
                        <v-btn
                            dark
                            color="indigo accent-2"
                            elevation="0"
                            class="rounded-lg me-2"
                            @click="savePembayaran()"
                        >Simpan</v-btn>
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
                    text: 'Pembayaran',
                    active: true,
                    href: '/projects/pembayaran/'
                },
                {
                    text: 'Detail Pembayaran',
                    active: false,
                },
            ],
            id: '',
            form: {},
            remove_dialog: {
                is_show: false
            },
            pembayaran_dialog: {
                is_show: false,
                data: {
                    nilai: '',
                    keterangan: ''
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
            this.$axios.get('/pembayarans/'+this.id)
                .then(res => {
                    this.form = res.data.data;
                })
        },
        done() {
            this.$router.push('/projects/pembayaran')
        },
        save(){
            let form_data = new FormData();
            form_data.append('status', this.form?.status)

            this.$axios.post('/kontraks/'+this.id, form_data)
                .then(res => {
                    console.log('success')
                })
        },
        closePembayaranDialog(){
            this.pembayaran_dialog = {
                is_show: false,
                data: {
                    nilai: '',
                    keterangan: ''
                }
            }
        },
        savePembayaran() {
            let form_data = new FormData();
            form_data.append('nominal', this.pembayaran_dialog.data.nilai)
            form_data.append('keterangan', this.pembayaran_dialog.data.keterangan)
            form_data.append('kontrak_id', this.id)
            this.$axios.post('/pembayarans/'+this.id, form_data)
                .then(res => {
                    this.load();
                    this.pembayaran_dialog = {
                        is_show: false,
                        data: {
                            nilai: '',
                            keterangan: ''
                        }
                    }
                })
        },
        remove() {
            this.$axios.delete('/pembayarans/'+this.id)
                .then(res => this.$router.push('/projects/pembayaran'))
        }
    }
}
</script>

<style>

</style>