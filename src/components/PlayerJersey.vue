<template>
    <v-flex xs6>
        <v-text-field
                :name="name"
                :value="input"
                :rules="playerJerseyRules"
                :counter="2"
                :label="label"
                required
        ></v-text-field>
    </v-flex>
</template>

<script>
    export default {
        name: "PlayerName",
        data: () => ({
            playerJerseyRules: [
                v => !!v || 'Player number is required',
                v => v.length <= 2 || 'Player name must be less than 2 characters',
            ],
            input: '',
        }),
        props: {
            label: {
                required: true,
                type: String
            },
            name: {
                required: true,
                type: String
            }
        }
    }
</script>

<style scoped>

</style>