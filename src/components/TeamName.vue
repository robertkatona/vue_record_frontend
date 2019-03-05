<template>
        <v-flex xs6>
            <v-text-field
                    :name="name"
                    :value="teamName"
                    :rules="teamNameRules"
                    :counter="15"
                    :label="label"
                    required
            ></v-text-field>
        </v-flex>

</template>

<script>
    export default {
        name: "TeamName",
        data: () => ({
            teamNameRules: [
                v => !!v || 'Team name is required',
                v => v.length <= 15 || 'Team name must be less than 20 characters',
            ],
            teamName: ''
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