<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
    <div id="list-players-table">
        <v-toolbar flat color="white">
            <v-toolbar-title>{{ teamName }}</v-toolbar-title>
            <v-divider
                    class="mx-2"
                    inset
                    vertical
            ></v-divider>
            <v-spacer></v-spacer>
        </v-toolbar>

        <template>
            <div>
                <v-data-table
                        :headers="headers"
                        :items="teamMembers"
                >
                    <template v-slot:items="props">
                        <td>
                            <v-edit-dialog
                                    :return-value.sync="props.item.name"
                                    lazy
                                    @save="save"
                                    @cancel="cancel"
                                    @open="open"
                                    @close="close"
                            > {{ props.item.name }}
                                <template v-slot:input>
                                    <v-text-field
                                            v-model="props.item.name"
                                            :rules="[maxNameChars]"
                                            label="Edit"
                                            single-line
                                            counter
                                    ></v-text-field>
                                </template>
                            </v-edit-dialog>
                        </td>
                        <td>
                            <v-edit-dialog
                                    :return-value.sync="props.item.jersey"
                                    lazy
                                    @save="save"
                                    @cancel="cancel"
                                    @open="open"
                                    @close="close"
                            > {{ props.item.jersey }}
                                <template v-slot:input>
                                    <v-text-field
                                            v-model="props.item.jersey"
                                            :rules="[maxJerseyChars]"
                                            label="Edit"
                                            single-line
                                            counter
                                    ></v-text-field>
                                </template>
                            </v-edit-dialog>
                        </td>
                        <td>
                            <v-checkbox  v-model="props.item.onTheCourt"
                                         primary
                                         hide-details
                            ></v-checkbox>
                        </td>
                    </template>
                </v-data-table>

                <v-snackbar v-model="snack" :timeout="3000" :color="snackColor">
                    {{ snackText }}
                    <v-btn flat @click="snack = false">Close</v-btn>
                </v-snackbar>
            </div>
        </template>
    </div>
</template>

<script>
    export default {
        props: ['teamName', 'teamMembers'],
        data() {
            return {
                snack: false,
                snackColor: '',
                snackText: '',
                maxNameChars: v => v.length <= 20 || 'Input too long!',
                maxJerseyChars: v => v.length <= 2 || 'Jersey must be max 2 digits long!',
                pagination: {},
                headers: [
                    {
                        text: 'Players',
                        align: 'left',
                        sortable: false,
                        value: 'name'
                    },
                    {text: '# Jersey', value: 'jersey'},
                    {text: 'Player On The Court', value: 'onTheCourt'}
                ],
            }
        },
        methods: {
            save() {
                this.snack = true;
                this.snackColor = 'success';
                this.snackText = 'Data saved'
            },
            cancel() {
                this.snack = true;
                this.snackColor = 'error';
                this.snackText = 'Canceled'
            },
            open() {
                this.snack = true;
                this.snackColor = 'info';
                this.snackText = 'Dialog opened'
            },
            close() {
                console.log('Dialog closed')
            }
        }
    }
</script>

<style scoped>
    #list-players-table {
        width: 50%;
        height: 100%;
        float: left;
    }
</style>