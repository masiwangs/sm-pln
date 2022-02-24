<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap Kontrak</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 1</div>
          <v-btn v-on:click="buatProjectBaru(1)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_1" item-key="id" :items-per-page="5" class="elevation-1 mb-4">
          <template v-slot:item.rab_jasa="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailMaterial(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_material) }}
            </a>
          </template>
        </v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 2</div>
          <v-btn v-on:click="buatProjectBaru(2)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_2" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template v-slot:item.rab_jasa="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailMaterial(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_material) }}
            </a>
          </template>
        </v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 3</div>
          <v-btn v-on:click="buatProjectBaru(3)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_3" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template v-slot:item.rab_jasa="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailMaterial(item.id, item.nama_project)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_material) }}
            </a>
          </template>
        </v-data-table>
      </v-card>
    </v-col>

    <v-dialog v-model="material_dialog.is_open" max-width="640px">
      <v-card>
        <v-card-title class="text-h5">
          Detail Material {{ material_dialog.nama_project }}
        </v-card-title>

        <v-card-text v-if="!material_dialog.loading">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Nama</th>
                  <th class="text-left">Jumlah</th>
                  <th class="text-left">Harga (Rp)</th>
                  <th class="text-left">Total (Rp)</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in material_dialog.data" :key="index">
                  <td>{{ item.nama }}</td>
                  <td>{{ item.jumlah }}</td>
                  <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.harga) }}</td>
                  <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.total) }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <v-progress-linear
            v-show="material_dialog.loading"
            indeterminate
            color="primary"
            class="mb-0"
        ></v-progress-linear>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn text @click="material_dialog.is_open = false" color="red">
            Edit
          </v-btn>

          <v-btn text @click="material_dialog.is_open = false" color="primary">
            Tutup
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="jasa_dialog.is_open" max-width="640px">
      <v-card>
        <v-card-title class="text-h5">
          Detail Jasa {{ jasa_dialog.nama_project }}
        </v-card-title>

        <v-card-text v-if="!jasa_dialog.loading">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Nama</th>
                  <th class="text-left">Nilai (Rp)</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in jasa_dialog.data" :key="index">
                  <td>{{ item.nama }}</td>
                  <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.nilai) }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <v-progress-linear
            v-show="jasa_dialog.loading"
            indeterminate
            color="primary"
            class="mb-0"
        ></v-progress-linear>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn text @click="jasa_dialog.is_open = false" color="red">
            Edit
          </v-btn>

          <v-btn text @click="jasa_dialog.is_open = false" color="primary">
            Tutup
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="project_baru_dialog.is_open"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card>
        <v-toolbar
          dark
          color="primary"
        >
          <v-btn
            icon
            dark
            @click="project_baru_dialog.is_open = false"
          >
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Buat Project Baru Basket {{ project_baru_dialog.basket }}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn
              dark
              text
              @click="project_baru_dialog.is_open = false"
            >
              Simpan
            </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-card-text class="pt-6">
          <v-row class="mb-3">
            <v-col cols="12">
              <h2>Informasi</h2>
            </v-col>
            <v-col cols="12" md="6">
              <v-autocomplete
                v-model="project_baru_dialog.prk"
                :items="skki_autocomplete_items"
                dense
                outlined
                label="Nota Dinas"
              ></v-autocomplete>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="No. PRK"
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="SKKI"
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="No. WBS"
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="Tgl. Berlaku"
                outlined
                dense
                type="date"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="Tgl. Awal"
                outlined
                dense
                type="date"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="Tgl. Akhir"
                outlined
                dense
                type="date"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row class="mb-3">
            <v-col cols="12">
              <h2>RAB Jasa</h2>
            </v-col>
            <v-col cols="12">
              <v-row>
                <v-col cols="4">
                  <h4>Jasa</h4>
                </v-col>
                <v-col cols="7">
                  <h4>Nilai</h4>
                </v-col>
                <v-col cols="1">
                  &nbsp;
                </v-col>
              </v-row>
              <v-row v-for="(item, index) in project_baru_dialog.list_jasa" :key="index">
                <v-col cols="4">
                  <p>{{ item.jasa }}</p>
                </v-col>
                <v-col cols="7">
                  <p>{{ item.nilai }}</p>
                </v-col>
                <v-col cols="1">
                  <v-btn color="red" v-on:click="hapusJasaProjectBaru(index)"><v-icon color="white">mdi-delete</v-icon></v-btn>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="4">
                  <v-text-field
                    label="Jasa"
                    outlined
                    dense
                    v-model="jasa_project_baru.jasa"
                  ></v-text-field>
                </v-col>
                <v-col cols="7">
                  <v-text-field
                    label="Nilai (Rp)"
                    outlined
                    dense
                    v-model="jasa_project_baru.nilai"
                  ></v-text-field>
                </v-col>
                <v-col cols="1">
                  <v-btn color="primary" v-on:click="tambahJasaProjectBaru()">
                    <v-icon>mdi-plus</v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-row>
                <v-col cols="12">
                  <h2>RAB Material</h2>
                </v-col>
                <v-col cols="12">
                  <v-row>
                    <v-col cols="3">
                      <h4>Nama</h4>
                    </v-col>
                    <v-col cols="4">
                      <h4>Jumlah</h4>
                    </v-col>
                    <v-col cols="4">
                      <h4>Harga</h4>
                    </v-col>
                    <v-col cols="1">
                      &nbsp;
                    </v-col>
                  </v-row>
                  <v-row v-for="(item, index) in project_baru_dialog.list_material" :key="index">
                    <v-col cols="3">
                      <p>{{ item.nama }}</p>
                    </v-col>
                    <v-col cols="4">
                      <p>{{ item.jumlah }}</p>
                    </v-col>
                    <v-col cols="4">
                      <p>{{ item.harga }}</p>
                    </v-col>
                    <v-col cols="1">
                      <v-btn color="red" v-on:click="hapusMaterialProjectBaru(index)"><v-icon color="white">mdi-delete</v-icon></v-btn>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="3">
                      <v-text-field
                        label="Nama"
                        outlined
                        dense
                        v-model="material_project_baru.nama"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field
                        label="Jumlah"
                        outlined
                        dense
                        v-model="material_project_baru.jumlah"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field
                        label="Harga"
                        outlined
                        dense
                        v-model="material_project_baru.harga"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="1">
                      <v-btn v-on:click="tambahMaterialProjectBaru()" color="primary">
                        <v-icon>mdi-plus</v-icon>
                      </v-btn>
                    </v-col>
                  </v-row>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  export default {
    components: {},
    data() {
      return {
        breadcrumbItems: [{
            text: 'Dashboard',
            active: true,
            href: '/'
          },
          {
            text: 'Project Tahap Kontrak',
            active: false,
          },
        ],

        tbl_header: [
            {
              text: 'Nama Project',
              value: 'nama_project'
            },
            {
              text: 'SKKI',
              value: 'skki'
            },
            {
              text: 'PRK',
              value: 'prk'
            },
            {
              text: 'WBS',
              value: 'wbs'
            },
            {
              text: 'Nota Dinas',
              value: 'wbs'
            },
            {
              text: 'Tgl. Berlaku',
              value: 'tgl_berlaku'
            },
            {
              text: 'Tgl. Awal',
              value: 'tgl_awal'
            },
            {
              text: 'Tgl. Akhir',
              value: 'tgl_akhir'
            },
            {
              text: 'RAB Jasa',
              value: 'rab_jasa'
            },
            {
              text: 'RAB Material',
              value: 'rab_material'
            },
        ],

        tbl_items_basket_1: [
          {
            id: 1,
            nama_project: 'Project A',
            skki: 'SKKI0101',
            prk: 'PRK0101',
            wbs: 'WBS0101',
            nota_dinas: 'NODIN0101',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            rab_jasa: 190000000,
            rab_material: 1000000000
          },
          {
            id: 2,
            skki: 'SKKI0102',
            prk: 'PRK0102',
            wbs: 'WBS0102',
            nota_dinas: 'NODIN0102',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            nama_project: 'Project B',
            rab_jasa: 180000000,
            rab_material: 2000000000
          },
        ],
        tbl_items_basket_2: [
          {
            id: 3,
            skki: 'SKKI0201',
            prk: 'PRK0201',
            wbs: 'WBS0201',
            nota_dinas: 'NODIN0201',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            nama_project: 'Project C',
            rab_jasa: 192000000,
            rab_material: 3000000000
          },
          {
            id: 4,
            skki: 'SKKI0202',
            prk: 'PRK0202',
            wbs: 'WBS0202',
            nota_dinas: 'NODIN0202',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            nama_project: 'Project D',
            rab_jasa: 181000000,
            rab_material: 2300000000
          },
        ],
        tbl_items_basket_3: [
          {
            id: 5,
            skki: 'SKKI0301',
            prk: 'PRK0301',
            wbs: 'WBS0301',
            nota_dinas: 'NODIN0301',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            nama_project: 'Project E',
            rab_jasa: 190000000,
            rab_material: 1000000000
          },
          {
            id: 6,
            skki: 'SKKI0302',
            prk: 'PRK0302',
            wbs: 'WBS0302',
            nota_dinas: 'NODIN0302',
            tgl_berlaku: '1-1-2022',
            tgl_awal: '1-1-2022',
            tgl_akhir: '1-2-2022',
            nama_project: 'Project F',
            rab_jasa: 184000000,
            rab_material: 2100000000
          },
        ],

        jasa_dialog: {
          is_open: false,
          loading: false,
          data: [],
          nama_project: ''
        },
        material_dialog: {
          is_open: false,
          loading: false,
          data: [],
          nama_project: ''
        },

        project_baru_dialog: {
          is_open: false,
          basket: 1,
          prk: '',
          list_jasa: [],
          list_material: []
        },

        material_project_baru: {
          nama: '',
          jumlah: '',
          harga: ''
        },

        jasa_project_baru: {
          jasa: '',
          nilai: ''
        },

        skki_autocomplete_items: ['NODIN0101', 'NODIN0102', 'NODIN0103', 'NODIN0104'],
      }
    },
    methods: {
      detailJasa(id, nama_project) {
        this.jasa_dialog.nama_project = nama_project
        this.jasa_dialog.is_open = true
        this.jasa_dialog.loading = true
        setTimeout(() => {
          this.jasa_dialog.data = [{
              nama: 'Jasa A',
              nilai: Math.ceil(Math.random(1, 9)*10)*1000000
            },
            {
              nama: 'Jasa B',
              nilai: Math.ceil(Math.random(1, 20)*10)*100000
            },
          ]
          this.jasa_dialog.loading = false
        }, 1000);
      },
      detailMaterial(id, nama_project) {
        this.material_dialog.nama_project = nama_project
        this.material_dialog.is_open = true
        this.material_dialog.loading = true
        setTimeout(() => {
          let jumlahA = Math.ceil(Math.random(1, 9) * 100);
          let jumlahB = Math.ceil(Math.random(1, 9) * 100);

          let hargaA = Math.ceil(Math.random(1, 9)*10)*1000000;
          let hargaB = Math.ceil(Math.random(1, 9)*10)*1000000;

          this.material_dialog.data = [{
              nama: 'Material A',
              jumlah: jumlahA,
              harga: hargaA,
              total: jumlahA*hargaA
            },
            {
              nama: 'Material B',
              jumlah: jumlahB,
              harga: hargaB,
              total: jumlahB*hargaB
            },
          ]
          this.material_dialog.loading = false
        }, 1000);
      },
      buatProjectBaru(basket) {
        this.project_baru_dialog.is_open = true
        this.project_baru_dialog.basket = basket
      },
      tambahMaterialProjectBaru() {
        this.project_baru_dialog.list_material.push(
          {
            nama: this.material_project_baru.nama,
            jumlah: this.material_project_baru.jumlah,
            harga: this.material_project_baru.harga,
          }
        )
        this.material_project_baru.nama = ''
        this.material_project_baru.jumlah = ''
        this.material_project_baru.harga = ''
      },
      hapusMaterialProjectBaru(i) {
        this.project_baru_dialog.list_material = this.project_baru_dialog.list_material.filter((item, index) => index !== i)
      },
      tambahJasaProjectBaru() {
        this.project_baru_dialog.list_jasa.push(
          {
            jasa: this.jasa_project_baru.jasa,
            nilai: this.jasa_project_baru.nilai
          }
        )
        this.jasa_project_baru.jasa = ''
        this.jasa_project_baru.nilai = ''
      },
      hapusJasaProjectBaru(i) {
        this.project_baru_dialog.list_jasa = this.project_baru_dialog.list_jasa.filter((item, index) => index !== i)
      }
    },
  }

</script>

<style>

</style>
