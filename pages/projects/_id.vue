<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div class="">
        <h4 style="margin-bottom: -.25rem" class="grey--text">Detail Project</h4>
        <h2>Project Pembangunan Jalan</h2>
      </div>
    </v-col>
    <v-col cols="12">
      <v-card elevation="2">
        <v-card-title>
          Informasi Project
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12">
              <v-text-field outlined dense label="Area" value="Jakarta"></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-textarea rows="3" outlined dense label="Deskripsi" value="Lorem ipsum dolor sit amet consectetur adipisicing elit. Assumenda nihil illo alias nam sed consectetur. Consequuntur, corrupti. Voluptates sapiente ipsam sequi et, dolores eligendi provident quibusdam est suscipit fuga ex?"></v-textarea>
            </v-col>
          </v-row>
          <div class="d-flex justify-end">
            <v-btn elevation="2" color="primary">
              <v-icon class="me-2">mdi-content-save</v-icon> Simpan
            </v-btn>
          </div>
        </v-card-text>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title>Progress</v-card-title>
        <v-card-text>
          <v-tabs
            background-color="blue accent-4"
            dark
            grow
            v-model="tab"
          >
            <v-tab v-for="(tab, index) in tabs" :key="index">
              <v-icon class="me-2">{{ tab.icon }}</v-icon>
              {{ tab.text }}
            </v-tab>
          </v-tabs>

          <v-tabs-items v-model="tab">
            <v-tab-item>
              <detail-prk/>
            </v-tab-item>

            <v-tab-item>
              <detail-skki/>
            </v-tab-item>

            <v-tab-item>
              <detail-pengadaan/>
            </v-tab-item>

            <v-tab-item>
              <detail-kontrak/>
            </v-tab-item>

            <v-tab-item>
              <detail-pelaksanaan/>
            </v-tab-item>

            <v-tab-item>
              <detail-pembayaran/>
            </v-tab-item>
          </v-tabs-items>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import DetailPrk from '~/components/project/DetailPrk.vue'
import DetailSkki from '~/components/project/DetailSkki.vue'
import DetailPengadaan from '~/components/project/DetailPengadaan.vue'
import DetailKontrak from '~/components/project/DetailKontrak.vue'
import DetailPelaksanaan from '~/components/project/DetailPelaksanaan.vue'
import DetailPembayaran from '~/components/project/DetailPembayaran.vue'
export default {
  components: {
    DetailPrk,
    DetailSkki,
    DetailPengadaan,
    DetailKontrak,
    DetailPelaksanaan,
    DetailPembayaran,
  },
  data() {
    return {
      breadcrumbItems: [
        {
          text: 'Dashboard',
          active: true,
          href: '/'
        },
        {
          text: 'Project',
          active: true,
          href: '/project'
        },
        {
          text: 'Detail',
          active: false,
        },
      ],
      tab: false,
      tabs: [
        {
          text: 'PRK',
          icon: 'mdi-puzzle'
        },
        {
          text: 'SKKI',
          icon: 'mdi-mail'
        },
        {
          text: 'Pengadaan',
          icon: 'mdi-cart'
        },
        {
          text: 'Kontrak',
          icon: 'mdi-handshake'
        },
        {
          text: 'Pelaksanaan',
          icon: 'mdi-crane'
        },
        {
          text: 'Pembayaran',
          icon: 'mdi-cash-multiple'
        },
      ],

    }
  }
}
</script>

<style>

</style>
