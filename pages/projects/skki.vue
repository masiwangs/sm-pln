<template>
  <v-row>
    <v-col cols="12">
      <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
      <div>
        <h4 style="margin-bottom: -.25rem" class="grey--text">List Project</h4>
        <h2>Tahap SKKI</h2>
      </div>
    </v-col>

    <v-col cols="12">
      <basket-card tahap="skki" :basket="1" @show_material="showMaterial" @show_jasa="showJasa" @new_project="newProject"/>
    </v-col>
    <v-col cols="12">
      <basket-card tahap="skki" :basket="2" @show_material="showMaterial" @show_jasa="showJasa" @new_project="newProject"/>
    </v-col>
    <v-col cols="12">
      <basket-card tahap="skki" :basket="3" @show_material="showMaterial" @show_jasa="showJasa" @new_project="newProject"/>
    </v-col>
    
    <material-list :is_show="material.is_show" :data="material.data" @hide_dialog="material.is_show = false"/>
    <jasa-list :is_show="jasa.is_show" :data="jasa.data" @hide_dialog="jasa.is_show = false"/>
    <skki-form ref="formSkki" :is_show="new_project.is_show" :basket="new_project.basket" @hide_dialog="new_project.is_show = false"/>
  </v-row>
</template>

<script>
  import BasketCard from '~/components/card/BasketCard.vue'
  import MaterialList from '~/components/dialog/MaterialList.vue'
  import JasaList from '~/components/dialog/JasaList.vue'
  import SkkiForm from '~/components/dialog/SkkiForm.vue'

  export default {
    components: {BasketCard, MaterialList, JasaList, SkkiForm},
    data() {
      return {
        breadcrumbItems: [{
            text: 'Dashboard',
            active: true,
            href: '/'
          },
          {
            text: 'Project Tahap SKKI',
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

        new_project: {
          is_show: false,
          basket: ''
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
      newProject(basket) {
        this.new_project.basket = basket
        this.new_project.is_show = true
        this.$refs.formSkki.justLog()
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
    },
  }
</script>

<style>

</style>
