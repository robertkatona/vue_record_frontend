<template>
    <v-flex xs6>
        <v-text-field
                :name="name"
                :value="input"
                :rules="playerNameRules"
                :counter="15"
                :label="label"
                required
        ></v-text-field>
    </v-flex>
</template>

<script>
    export default {
        name: "PlayerName",
        data: () => ({
            playerNameRules: [
                v => !!v || 'Player name is required',
                v => v.length <= 15 || 'Player name must be less than 15 characters',
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