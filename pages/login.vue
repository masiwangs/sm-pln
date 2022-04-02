<template>
    <v-container>
        <v-form>
            <v-row>
                <v-col cols="12">
                    <h1>Admin Login</h1>
                    <p>Masukkan alamat email dan password yang valid,</p>
                </v-col>
                <v-col cols="12">
                    <v-text-field
                    label="Alamat email"
                    outlined
                    :hide-details="true"
                    ></v-text-field>
                </v-col>
                <v-col cols="12">
                    <v-text-field
                    label="Password"
                    outlined
                    :hide-details="true"
                    ></v-text-field>
                    <small class="grey--text">Lupa password? <NuxtLink to="home">Reset</NuxtLink></small>
                </v-col>
                <v-col cols="12">
                    <v-btn
                    large
                    block
                    color="indigo accent-2"
                    class="white--text"
                    >Masuk</v-btn>
                </v-col>
            </v-row>
        </v-form>
        <v-divider class="my-6"></v-divider>
        <div class="text-center">
            Belum punya akun? <NuxtLink :to="'home'">Registrasi</NuxtLink>
        </div>
    </v-container>
</template>

<script>
export default {
    layout: 'auth'
}
</script>

<style>

</style>