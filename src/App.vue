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
                    <v-btn type="submit" outline color="green">Save</v-btn>
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
                            <PlayerName name="inputName" label="Player name"></PlayerName>
                            <PlayerJersey name="inputJersey" label="# Jersey"></PlayerJersey>
                        </v-layout>
                    </v-container>

                    <template>
                        <v-card height="200px" flat>
                            <div class="headline text-xs-center pa-5">
                                {{ getTeams() }}
                            </div>
                            <v-bottom-nav
                                    :value="true"
                                    absolute
                                    color="transparent"
                            >
                                <v-btn
                                        color="teal"
                                        flat
                                        :value="teams.homeTeam"
                                >
                                    <span>{{ teams.homeTeam }}</span>
                                    <v-icon>person_outline</v-icon>
                                </v-btn>

                                <v-btn
                                        color="teal"
                                        flat
                                        :value="teams.visitorTeam"
                                        v-model="switch1"
                                >
                                    <span>{{ teams.visitorTeam }}</span>
                                    <v-icon>person</v-icon>
                                </v-btn>

                            </v-bottom-nav>
                        </v-card>
                    </template>

                    <v-btn type="submit" outline color="green">Save</v-btn>
                </v-form>
            </div>
            <div class="team-table">
                <TeamEditor :team-members="teamMembers.homeTeamPlayers" :team-name="teams.homeTeam"></TeamEditor>
            </div>
            <div class="team-table">
                <TeamEditor :team-members="teamMembers.visitorTeamPlayers" :team-name="teams.visitorTeam"></TeamEditor>
            </div>
        </div>
    </div>
</template>

<script>
    import TeamEditor from "./components/TeamEditor.vue";
    import TeamName from "./components/TeamName.vue";
    import PlayerName from "./components/PlayerName";
    import PlayerJersey from "./components/PlayerJersey";


    export default {
        components: {
            PlayerName,
            PlayerJersey,
            TeamEditor,
            TeamName
        },
        data: () => ({
            defaultTeamName: 'Team Name',
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
            editedIndex: -1,
            editedPlayer: {
                name: '',
                jersey: '',
                team: '',
                onTheCourt: false
            },
            defaultIPlayer: {
                firstName: '',
                lastName: '',
                jersey: '',
                team: '',
                onTheCourt: false
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
                    console.log(this.teams.homeTeam);
                    return this.teams.homeTeam;
                }
                console.log(this.teams.visitorTeam);
                return this.teams.visitorTeam;

            },
            getTeamNames: function (submitEvent) {
                this.teams.homeTeam = submitEvent.target.elements.home.value;
                this.teams.visitorTeam = submitEvent.target.elements.visitor.value;
                // console.log('Home: ' + this.teams.homeTeam);
                // console.log('Visitor: ' + this.teams.visitorTeam);
            },
            addNewPlayer: function (submitEvent) {
                this.editedPlayer.name = submitEvent.target.elements.inputName.value;
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
