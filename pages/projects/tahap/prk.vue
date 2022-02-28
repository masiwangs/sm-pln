<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div>
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap PRK</h2>
      </div>
    </v-col>

    <v-col cols="12">
      <tabel-basket :no_basket="1" @show_material="showMaterial" @show_jasa="showJasa"/>
    </v-col>

    <v-col cols="12">
      
    </v-col>
    <v-col cols="12">
      
    </v-col>
    
    <material-list :is_show="material.is_show" :data="material.data" @hide_dialog="material.is_show = false"/>
    <jasa-list :is_show="jasa.is_show" :data="jasa.data" @hide_dialog="jasa.is_show = false"/>

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
  import TabelBasket from '~/components/project/tahap/TabelBasket.vue'
  import MaterialList from '~/components/dialog/MaterialList.vue'
  import JasaList from '~/components/dialog/JasaList.vue'
  export default {
    components: {TabelBasket, MaterialList, JasaList},
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

        material: {
          is_show: false,
          data: []
        },

        jasa: {
          is_show: false,
          data: []
        },

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
        },
        count: 0
      }
    },
    methods: {
      showMaterial(data) {
        this.material.data = data
        this.material.is_show = true
      },
      showJasa(data) {
        this.jasa.data = data
        this.jasa.is_show = true
      },
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
      },

      increaseCount(n) {
        this.count += n
        console.log(this.count)
      }
    },
  }
</script>

<style>

</style>
