<template>
    <v-dialog v-model="is_show" max-width="290">
        <v-card>
            <v-date-picker v-model="dates" range></v-date-picker>
            <v-card-text class="d-flex justify-end">
                <v-btn color="primary" @click="saveDate">OK</v-btn>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: ['is_show'],
    data() {
        return {
            dates: []
        }
    },
    methods: {
        saveDate() {
            this.$emit('save_date', this.dates)
            this.$emit('hide_dialog')
        }
    }
}
</script>

<style>

</style>