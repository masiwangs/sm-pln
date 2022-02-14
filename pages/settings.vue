<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
        <div class="d-flex justify-space-between">
          <div class="">
            <h2>Pengaturan</h2>
          </div>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>Periode Project</v-card-title>
          <v-card-text>
            Aktif periode Project
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>Admin</v-card-title>
          <v-card-text>
            Aktif periode Project
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        breadcrumbItems: [{
            text: 'Dashboard',
            active: true,
            href: '/'
          },
          {
            text: 'Pengaturan',
            active: false,
          },
        ],
      }
    }
  }
</script>

<style>

</style>