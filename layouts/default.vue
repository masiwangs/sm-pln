<template>
    <v-app dark>
        <v-navigation-drawer
            v-model="drawer"
            clipped
            fixed
            app
            class="px-4 rounded-r-lg"
        >
            <v-list
                rounded
            >
                <v-list-item
                    to="/"
                    router
                    exact
                    color="indigo"
                >
                    <v-list-item-icon>
                        <v-icon>mdi-desktop-mac-dashboard</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>Dashboard</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
                <v-subheader>Tahapan</v-subheader>
                <v-list-item
                    v-for="([title, link, icon], i) in admins"
                    :key="i"
                    :to="'/projects/'+link"
                    router
                    exact
                    link
                    color="indigo"
                >
                <v-list-item-action>
                    <v-icon>mdi-{{ icon }}</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                    <v-list-item-title>{{ title }}</v-list-item-title>
                </v-list-item-content>
                </v-list-item>
                <v-subheader>Database</v-subheader>
                <v-list-item
                    to="/materials"
                    router
                    exact
                    color="indigo"
                >
                    <v-list-item-icon>
                        <v-icon>mdi-package-variant-closed</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>Material</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-main class="indigo lighten-5">
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
        ['PRK', 'prk', 'puzzle'],
        ['SKKI', 'skki', 'file'],
        ['Pengadaan', 'pengadaan', 'cart'],
        ['Kontrak', 'kontrak', 'file-sign'],
        ['Pelaksanaan', 'pelaksanaan', 'crane'],
        ['Pembayaran', 'pembayaran', 'cash'],

      ],
      title: 'Monitoring'
    }
  }
}
</script>
