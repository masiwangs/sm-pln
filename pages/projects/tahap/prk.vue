<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap PRK</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 1</div>
          <v-btn v-on:click="buatProjectBaru(1)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_1" item-key="id" :items-per-page="5"
          class="elevation-1 mb-4">
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
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_2" item-key="name" :items-per-page="5"
          class="elevation-1 mb-4">
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
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_3" item-key="name" :items-per-page="5"
          class="elevation-1 mb-4">
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
    <v-dialog v-model="material_dialog" max-width="640px">
      <v-card>
        <v-card-title class="text-h5">
          Detail Material {{ detail_material_value.nama_project }}
        </v-card-title>

        <v-card-text v-if="!detail_material_value.loading">
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
                <tr v-for="(item, index) in detail_material_value.data" :key="index">
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
            v-show="detail_material_value.loading"
            indeterminate
            color="primary"
            class="mb-0"
        ></v-progress-linear>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn text @click="material_dialog = false" color="red">
            Edit
          </v-btn>

          <v-btn text @click="material_dialog = false" color="primary">
            Tutup
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="jasa_dialog" max-width="640px">
      <v-card>
        <v-card-title class="text-h5">
          Detail Jasa {{ detail_jasa_value.nama_project }}
        </v-card-title>

        <v-card-text v-if="!detail_jasa_value.loading">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Nama</th>
                  <th class="text-left">Nilai (Rp)</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in detail_jasa_value.data" :key="index">
                  <td>{{ item.nama }}</td>
                  <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.nilai) }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <v-progress-linear
            v-show="detail_jasa_value.loading"
            indeterminate
            color="primary"
            class="mb-0"
        ></v-progress-linear>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn text @click="jasa_dialog = false" color="red">
            Edit
          </v-btn>

          <v-btn text @click="jasa_dialog = false" color="primary">
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
              <v-text-field
                label="Nama Project"
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="No. PRK"
                outlined
                dense
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
            text: 'Project Tahap PRK',
            active: false,
          },
        ],

        tbl_header: [{
            text: 'Nama Project',
            value: 'nama_project'
          },
          {
            text: 'PRK',
            value: 'prk'
          },
          {
            text: 'RAB Jasa (Rp)',
            value: 'rab_jasa'
          },
          {
            text: 'RAB Material (Rp)',
            value: 'rab_material'
          },
        ],

        tbl_items_basket_1: [{
            id: 1,
            prk: 'PRK0101',
            nama_project: 'Project A',
            rab_jasa: 190000000,
            rab_material: 1000000000
          },
          {
            id: 2,
            prk: 'PRK0102',
            nama_project: 'Project B',
            rab_jasa: 180000000,
            rab_material: 2000000000
          },
        ],
        tbl_items_basket_2: [{
            id: 3,
            prk: 'PRK0201',
            nama_project: 'Project C',
            rab_jasa: 192000000,
            rab_material: 3000000000
          },
          {
            id: 4,
            prk: 'PRK0202',
            nama_project: 'Project D',
            rab_jasa: 181000000,
            rab_material: 2300000000
          },
        ],
        tbl_items_basket_3: [{
            id: 5,
            prk: 'PRK0301',
            nama_project: 'Project E',
            rab_jasa: 190000000,
            rab_material: 1000000000
          },
          {
            id: 6,
            prk: 'PRK0302',
            nama_project: 'Project F',
            rab_jasa: 184000000,
            rab_material: 2100000000
          },
        ],

        jasa_dialog: false,
        material_dialog: false,

        detail_jasa_value: {
          nama_project: '',
          data: [],
          loading: true
        },
        detail_material_value: {
          nama_project: '',
          data: [],
          loading: true
        },
        project_baru_dialog: {
          is_open: false,
          basket: 1,
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
        }
      }
    },
    methods: {
      detailJasa(id, nama_project) {
        this.detail_jasa_value.nama_project = nama_project
        this.jasa_dialog = true
        this.detail_jasa_value.loading = true
        setTimeout(() => {
          this.detail_jasa_value.data = [{
              nama: 'Jasa A',
              nilai: Math.ceil(Math.random(1, 9)*10)*1000000
            },
            {
              nama: 'Jasa B',
              nilai: Math.ceil(Math.random(1, 20)*10)*100000
            },
          ]
          this.detail_jasa_value.loading = false
        }, 1000);
      },
      detailMaterial(id, nama_project) {
        this.detail_material_value.nama_project = nama_project
        this.material_dialog = true
        this.detail_material_value.loading = true
        setTimeout(() => {
          let jumlahA = Math.ceil(Math.random(1, 9) * 100);
          let jumlahB = Math.ceil(Math.random(1, 9) * 100);

          let hargaA = Math.ceil(Math.random(1, 9)*10)*1000000;
          let hargaB = Math.ceil(Math.random(1, 9)*10)*1000000;

          this.detail_material_value.data = [{
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
          this.detail_material_value.loading = false
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
