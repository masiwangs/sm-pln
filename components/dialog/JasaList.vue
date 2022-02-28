<template>
  <v-dialog v-model="is_show" max-width="640px">
    <v-card>
      <v-card-title class="text-h5">
        Detail Jasa
      </v-card-title>

      <v-card-text>
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Nama</th>
                <th class="text-left">Nilai (Rp)</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in data" :key="index">
                <td>{{ item.nama }}</td>
                <td>Rp{{ new Intl.NumberFormat('id-ID').format(item.nilai) }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-card-text>
      

      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn text @click="$emit('hide_dialog')" color="red">
          Edit
        </v-btn>

        <v-btn text @click="$emit('close-dialog')" color="primary">
          Tutup
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
  export default {
    props: ['is_show', 'data']
  }
</script>

<style>

</style>