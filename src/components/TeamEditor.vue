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
        <v-data-table
                :headers="headers"
                :items="teamMembers"
                class="elevation-2"
        >
            <template slot="items" slot-scope="props">
                <td></td>
                <td class="text-xs-left">{{ props.item.firstName }}</td>
                <td class="text-xs-left">{{ props.item.lastName }}</td>
                <td class="text-xs-left"># {{ props.item.jersey }}</td>
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
        name: "TeamEditor",
        props: ['teamName', 'teamMembers'],
        data: () => ({
            valid: false,
            fill: true,
            teamType: 'Team',
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
                {text: '#Number', value: 'jersey'}
            ],
            editedIndex: -1,
            editedPlayer: {
                input: '',
                lastName: '',
                jersey: '',
                team: ''
            },
            defaultIPlayer: {
                input: '',
                lastName: '',
                jersey: '',
                team: ''
            }
        }),
        created() {
            this.initialize()
        },
        methods: {
            initialize() {
            },
            editItem(item) {
                console.log(this.teamMembers);
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
                    this.editedPlayer.team = this.teamName;
                    console.log(this.editedPlayer);
                    this.teamMembers.push(this.editedPlayer);
                }
                this.close()
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