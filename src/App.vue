<template>
    <div class="app">
        <div class="text-xs-center">
            <v-btn outline color="red">Start</v-btn>
            <div>
                <v-form @submit.prevent="getTeamNames" v-model="valid">
                    <v-container fill-height>
                        <v-layout>
                            <TeamName name="home" label="Home Team"></TeamName>
                            <TeamName name="visitor" label="Visitor Team"></TeamName>
                        </v-layout>
                    </v-container>
                    <v-btn type="submit" outline color="green">Submit</v-btn>
                </v-form>
            </div>
            <div>
                <br>
                <hr>
                <br>
            </div>
            <div>
                <v-form @submit.prevent="addNewPlayer" v-model="valid">
                    <v-container fill-height>
                        <v-layout>
                            <PlayerData name="inputFirstName" label="First name"></PlayerData>
                            <PlayerData name="inputLastName" label="Last name"></PlayerData>
                            <PlayerData name="inputJersey" label="# Jersey"></PlayerData>
                        </v-layout>
                        <v-container>
                            <v-switch v-model="switch1" :label="`TEAM: ${getTeams()}`"></v-switch>
                        </v-container>
                    </v-container>
                    <v-btn type="submit" outline color="green">Save</v-btn>
                </v-form>
            </div>
            <div class="team-table">
                <TeamEditor :team-name="teams.homeTeam"></TeamEditor>
            </div>
            <div class="team-table">
                <TeamEditor :team-name="teams.visitorTeam"></TeamEditor>
            </div>
        </div>
    </div>
</template>

<script>
    import TeamEditor from "./components/TeamEditor.vue";
    import TeamName from "./components/TeamName.vue";
    import PlayerData from "./components/PlayerData";


    export default {
        components: {
            PlayerData,
            TeamEditor,
            TeamName
        },
        data: () => ({
            switch1: false,
            teamName: '',
            teams: {
                homeTeam: '',
                visitorTeam: ''
            },
            teamMembers: {
                homeTeamPlayers: [],
                visitorTeamPlayers: [],

            },
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
                {text: 'First Name', value: 'input'},
                {text: 'Last Name', value: 'lastName'},
                {text: '#Number', value: 'jersey'}
            ],
            editedIndex: -1,
            editedPlayer: {
                firstName: '',
                lastName: '',
                jersey: '',
                team: ''
            },
            defaultIPlayer: {
                firstName: '',
                lastName: '',
                jersey: '',
                team: ''
            }
        }),

        computed: {
            formTitle() {
                return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
            }
        },

        watch: {
            dialog(val) {
                val || this.close()
            }
        },
        methods: {
            getTeams: function () {
                if (!this.switch1) {
                    return this.teams.homeTeam;
                }
                return this.teams.visitorTeam;

            },
            getTeamNames: function (submitEvent) {
                this.teams.homeTeam = submitEvent.target.elements.home.value;
                this.teams.visitorTeam = submitEvent.target.elements.visitor.value;
                // console.log('Home: ' + this.teams.homeTeam);
                // console.log('Visitor: ' + this.teams.visitorTeam);
            },
            addNewPlayer: function (submitEvent) {
                this.editedPlayer.firstName = submitEvent.target.elements.inputFirstName.value;
                this.editedPlayer.lastName = submitEvent.target.elements.inputLastName.value;
                this.editedPlayer.jersey = submitEvent.target.elements.inputJersey.value;
                if (!this.switch1) {
                    this.editedPlayer.team = this.teams.homeTeam;
                    this.teamMembers.homeTeamPlayers.push({...this.editedPlayer});
                    console.log(this.teamMembers);
                    return
                }
                this.editedPlayer.team = this.teams.visitorTeam;
                this.teamMembers.visitorTeamPlayers.push({...this.editedPlayer});
                console.log(this.teamMembers);
            }
        }
    }
</script>

<style>
    .team-table {
        margin: 5px auto;
    }
</style>