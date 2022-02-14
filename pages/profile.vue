<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-breadcrumbs :items="breadcrumbItems" class="px-0"></v-breadcrumbs>
        <div class="d-flex justify-space-between">
          <div class="">
            <h2>Akun Anda</h2>
          </div>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="3">
        <v-img
            :aspect-ratio="1/1"
            src="https://cdn.vuetifyjs.com/images/parallax/material.jpg"
        ></v-img>
      </v-col>
      <v-col cols="12" md="9">
          <v-card>
              <v-card-title>Ubah Profil Anda</v-card-title>
              <v-card-text>
                  <v-form>
                      
                  </v-form>
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
            text: 'Akun Anda',
            active: false,
          },
        ],
      }
    }
  }
</script>

<style>

</style>