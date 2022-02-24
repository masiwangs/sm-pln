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
        <v-data-table :headers="tableProjects" :items="tableProjectItems" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template
            v-slot:item.name="{ item }"
          >
            <NuxtLink :to="`/project/${item.name}`">{{ item.name }}</NuxtLink>
          </template>
          <template
            v-slot:item.carbs="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
          <template
            v-slot:item.protein="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
        </v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 2</div>
          <v-btn>Project Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tableProjects" :items="tableProjectItems" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template
            v-slot:item.name="{ item }"
          >
            <NuxtLink :to="`/project/${item.name}`">{{ item.name }}</NuxtLink>
          </template>
          <template
            v-slot:item.carbs="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
          <template
            v-slot:item.protein="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
        </v-data-table>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card>
        <v-card-title class="d-flex justify-space-between">
          <div>Basket 3</div>
          <v-btn>Project Baru</v-btn>
        </v-card-title>
        <v-data-table :headers="tableProjects" :items="tableProjectItems" item-key="name" :items-per-page="5" class="elevation-1 mb-4">
          <template
            v-slot:item.name="{ item }"
          >
            <NuxtLink :to="`/project/${item.name}`">{{ item.name }}</NuxtLink>
          </template>
          <template
            v-slot:item.carbs="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
          <template
            v-slot:item.protein="{ item }"
          >
            Rp{{ new Intl.NumberFormat('id-ID').format(item.carbs) }},00
          </template>
        </v-data-table>
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
        tableSearch: '',
        tableProjects: [{
            text: 'Nama Project',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          {
            text: 'No. PRK',
            value: 'skki'
          },
          {
            text: 'No. SKKI',
            value: 'skki'
          },
          {
            text: 'No. SPK',
            value: 'spk'
          },
          {
            text: 'RAB Jasa (Rp)',
            value: 'jasa'
          },
          {
            text: 'RAB Material (Rp)',
            value: 'material'
          },
          {
            text: 'Tahap',
            value: 'tahap'
          },
        ],
        tableProjectItems: [
        ],
      }
    },
    mounted() {
      let query = this.$route.query
    },
  }

</script>

<style>

</style>
