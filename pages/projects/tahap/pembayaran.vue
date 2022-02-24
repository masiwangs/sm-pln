<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap Pembayaran</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 1</div>
          <v-btn v-on:click="buatProjectBaru(3)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_1" item-key="id" :items-per-page="5" class="elevation-1 mb-4">
          <template v-slot:item.rab_jasa="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
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
          <v-btn v-on:click="buatProjectBaru(3)">Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tbl_header" :items="tbl_items_basket_2" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template v-slot:item.rab_jasa="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
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
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_jasa) }}
            </a>
          </template>
          <template v-slot:item.rab_material="{ item }">
            <a href="javascript:void(0)" v-on:click="detailJasa(item.id)">
              Rp{{ new Intl.NumberFormat('id-ID').format(item.rab_material) }}
            </a>
          </template>
        </v-data-table>
      </v-card>
    </v-col>

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
                label="No. SPK"
              ></v-autocomplete>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="BASTP"
                outlined
                dense
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                label="SE"
                outlined
                dense
              ></v-text-field>
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
            text: 'Project Tahap Pembayaran',
            active: false,
          },
        ],

        tbl_header: [
            {
              text: 'SPK',
              value: 'spk'
            },
            {
              text: 'BASTP',
              value: 'bastp'
            },
            {
              text: 'SE',
              value: 'se'
            },
        ],

        tbl_items_basket_1: [
          {
            id: 1,
            spk: 'SPK-1-1',
            bastp: 'BASTP0101',
            se: 'SE0101'
          },
          {
            id: 2,
            spk: 'SPK-1-2',
            bastp: 'BASTP0102',
            se: 'SE0102'
          },
        ],
        tbl_items_basket_2: [
          {
            id: 3,
            spk: 'SPK-2-1',
            bastp: 'BASTP0201',
            se: 'SE0201'
          },
          {
            id: 4,
            spk: 'SPK-2-2',
            bastp: 'BASTP0202',
            se: 'SE0202'
          },
        ],
        tbl_items_basket_3: [
          {
            id: 5,
            spk: 'SPK-3-1',
            bastp: 'BASTP0301',
            se: 'SE0301'
          },
          {
            id: 6,
            spk: 'SPK-3-1',
            bastp: 'BASTP0302',
            se: 'SE0302'
          },
        ],

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

        skki_autocomplete_items: ['SPK0101', 'SPK0102', 'SPK0103', 'SPK0104'],
      }
    },
    methods: {
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
