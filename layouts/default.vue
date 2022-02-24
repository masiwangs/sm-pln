<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      clipped
      fixed
      app
    >
      <v-list>
        <v-list-item
          to="/"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>mdi-desktop-mac-dashboard</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-group :value="false" no-action prepend-icon="mdi-crane">
          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title>List Project</v-list-item-title>
            </v-list-item-content>
          </template>

          <v-list-item v-for="([title, link], i) in admins" :key="i" :to="'/projects/tahap/'+link" router exact link>
            <v-list-item-title>{{ title }}</v-list-item-title>
          </v-list-item>
        </v-list-group>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      clipped-left
      fixed
      app
      dark
      color="primary"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn
        icon
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      drawer: true,
      admins: [
        ['PRK', 'prk'],
        ['SKKI', 'skki'],
        ['Pengadaan', 'pengadaan'],
        ['Kontrak', 'kontrak'],
        ['Pelaksanaan', 'pelaksanaan'],
        ['Pembayaran', 'pembayaran'],

      ],
      title: 'Monitoring'
    }
  }
}
</script>
