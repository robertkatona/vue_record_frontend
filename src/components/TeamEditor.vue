<template>
    <div>
        <v-toolbar flat color="white">
            <v-toolbar-title>{{ teamName }}</v-toolbar-title>
            <v-divider
                    class="mx-2"
                    inset
                    vertical
            ></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
                <v-btn slot="activator" color="primary" dark class="mb-2">New Player</v-btn>
                <v-card>
                    <v-card-title>
                        <span class="headline">Team: {{ teamName }}</span>
                    </v-card-title>

                    <v-card-text>
                        <v-container grid-list-md>
                            <v-layout wrap>
                                <v-flex xs12 sm6 md4>
                                    <v-text-field v-model="editedPlayer.firstName" label="First name"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <v-text-field v-model="editedPlayer.lastName" label="Last name"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <v-text-field v-model="editedPlayer.number" label="# Number"></v-text-field>
                                </v-flex>
                            </v-layout>
                        </v-container>
                    </v-card-text>

                    <v-card-actions height="500px">
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" flat @click="close">Cancel</v-btn>
                        <v-btn color="blue darken-1" flat @click="save">Save</v-btn>
                    </v-card-actions>
                </v-card>
            </v-dialog>
        </v-toolbar>
        <v-data-table
                :headers="headers"
                :items="homeTeamMembers"
                class="elevation-1"
        >
            <template slot="items" slot-scope="props">
                <td></td>
                <td class="text-xs-left">{{ props.item.firstName }}</td>
                <td class="text-xs-left">{{ props.item.lastName }}</td>
                <td class="text-xs-left"># {{ props.item.number }}</td>
                <td class="justify-center layout px-0">
                    <v-icon
                            small
                            class="mr-2"
                            @click="editItem(props.item)"
                    >
                        edit
                    </v-icon>
                    <v-icon
                            small
                            @click="deleteItem(props.item)"
                    >
                        delete
                    </v-icon>
                </td>
            </template>
            <template slot="no-data">
                <v-btn color="primary" @click="initialize">Reset</v-btn>
            </template>
        </v-data-table>
    </div>
</template>

<script>
    export default {
        name: "TeamEditor"  ,
        props: ['teamName'],
        data: () => ({
            teamName: '',
            valid: false,
            fill: true,
            teamType: 'Team',
            teamMembers: [],
            homeTeamMembers: [],
            visitorTeamMembers: [],
            nameRules: [
                v => !!v || 'Name is required',
                v => v.length <= 15 || 'Name must be less than 15 characters',

            ],
            dialog: false,
            headers: [
                {
                    text: 'Players',
                    align: 'left',
                    sortable: false,
                    value: 'name'
                },
                {text: 'First Name', value: 'firstName'},
                {text: 'Last Name', value: 'lastName'},
                {text: '#Number', value: 'number'}
            ],
            editedIndex: -1,
            editedPlayer: {
                firstName: '',
                lastName: '',
                number: '',
                team:''
            },
            defaultIPlayer: {
                firstName: '',
                lastName: '',
                number: '',
                team:''
            }
        }),
        created() {
            this.initialize()
        },
        methods: {
            initialize() {
                this.homeTeamMembers = [
                    {
                        firstName: 'Robert',
                        lastName: 'Katona',
                        number: 5
                    }],
                    this.visitorTeamMembers = [
                        {
                            firstName: 'Aaron',
                            lastName: 'Moravcsik',
                            number: 6
                        }
                    ]
            },
            editItem(item) {
                this.editedIndex = this.teamMembers.indexOf(item);
                this.editedPlayer = Object.assign({}, item);
                this.dialog = true
            },

            deleteItem(item) {
                const index = this.teamMembers.indexOf(item);
                confirm('Are you sure you want to delete this item?') && this.teamMembers.splice(index, 1)
            },

            close() {
                this.dialog = false;
                setTimeout(() => {
                    this.editedPlayer = Object.assign({}, this.defaultIPlayer);
                    this.editedIndex = -1
                }, 300)
            },

            save() {
                if (this.editedIndex > -1) {
                    Object.assign(this.teamMembers[this.editedIndex], this.editedPlayer);
                } else {
                    this.teamMembers.push(this.editedPlayer);
                    // console.log(this.teamMembers);
                }
                this.close()
            }
        }
    }
</script>

<style scoped>

</style>