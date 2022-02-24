<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">List</h4>
        <h2>Project</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 1</div>
          <v-btn>Project Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tblHeader" :items="tblBasket1" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          
        </v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 2</div>
          <v-btn>Project Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tblHeader" :items="tblBasket2" item-key="name" :items-per-page="5" class="elevation-1 mb-4"></v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 3</div>
          <v-btn>Project Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tblHeader" :items="tblBasket3" item-key="name" :items-per-page="5" class="elevation-1 mb-4"></v-data-table>
      </v-card>
    </v-col>
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
            text: 'Project',
            active: false,
          },
        ],

        tblHeader: [],
        tblItems: [],
        tblBasket1: [],
        tblBasket2: [],
        tblBasket3: [],

        header: {
          prk: [
            {
              text: 'Nama Project',
              value: 'nama_project'
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
          skki: [
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
              text: 'RAB Jasa',
              value: 'rab_jasa'
            },
            {
              text: 'RAB Material',
              value: 'rab_material'
            },
          ],
          pengadaan: [
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
              text: 'RAB Jasa',
              value: 'rab_jasa'
            },
            {
              text: 'RAB Material',
              value: 'rab_material'
            },
          ],
          kontrak: [
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
          pelaksanaan: [
            {
              text: 'Nama Project',
              value: 'nama_project'
            },
            {
              text: 'SPK',
              value: 'spk'
            },
            {
              text: 'Jasa',
              value: 'jasa'
            },
            {
              text: 'Material',
              value: 'material'
            },
          ],
          pembayaran: [
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
        },
      }
    },
    mounted() {
      let tahap = this.$route.params.tahap
      this.loadTable(tahap)
    },
    methods: {
      loadTable(tahap) {
        switch (tahap) {
          case 'prk':
            this.tblHeader = this.header.prk
            this.tblBasket1 = [
              {
                nama_project: 'Project X',
                rab_jasa: 1900000000,
                rab_material: 2100000000
              }
            ]
            break;

          case 'skki':
            this.tblHeader = this.header.skki
            break;

          case 'skki':
            this.tblHeader = this.header.skki
            break;

          case 'pengadaan':
            this.tblHeader = this.header.pengadaan
            break;

          case 'kontrak':
            this.tblHeader = this.header.kontrak
            break;

          case 'pelaksanaan':
            this.tblHeader = this.header.pelaksanaan
            break;

          default:
            this.tblHeader = this.header.prk
            break;
        }
      }
    }
  }

</script>

<style>

</style>
