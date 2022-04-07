<template>
    <v-dialog v-model="show" max-width="290">
        <v-card>
            <v-date-picker v-model="date"></v-date-picker>
            <v-card-text class="d-flex justify-end">
                <v-btn color="primary" @click="saveDate">OK</v-btn>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: ['show'],
    data() {
        return {
            date: ''
        }
    },
    methods: {
        saveDate() {
            this.$emit('save_date', this.date)
            this.$emit('hide_date_dialog')
        }
    }
}
</script>

<style>

</style>