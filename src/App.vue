<template>
    <div class="app">
        <div class="text-xs-center">
            <v-btn outline color="red">Start</v-btn>
            <v-form @submit.prevent="getData" v-model="valid">
                <v-container fill-height>
                    <v-layout>
                        <v-flex xs6>
                            <v-text-field
                                    name="home"
                                    v-model="homeTeam"
                                    :rules="teamNameRules"
                                    :counter="20"
                                    label="Home team"
                                    required
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs6>
                            <v-text-field
                                    name="visitor"
                                    v-model="visitorTeam"
                                    :rules="teamNameRules"
                                    :counter="20"
                                    label="Visitor Team"
                                    required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                </v-container>
                <v-btn type="submit" outline color="green">Submit</v-btn>
            </v-form>
            <v-form class="addMember" @submit.prevent="getMemberData">
                <v-container fill-height>
                    <v-layout>
                        <v-flex xs6>
                            <v-text-field
                                    name="firstName"
                                    v-model="firstName"
                                    :rules="nameRules"
                                    :counter="15"
                                    label="First Name"
                                    required
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs6>
                            <v-text-field
                                    name="lastName"
                                    v-model="lastName"
                                    :rules="nameRules"
                                    :counter="15"
                                    label="Last Name"
                                    required
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs2>
                            <v-switch
                                    :label="`${teamType}`"
                                    name="teamType"
                                    v-model="fill"
                                    @change="homeOrAway"
                                    color="black"
                            ></v-switch>
                        </v-flex>
                        <v-flex xs3>
                            <v-btn @submit.prevent="getMemberData" type="submit" outline color="blue">Add member</v-btn>
                        </v-flex>
                    </v-layout>
                </v-container>
            </v-form>
            <v-container>
                <v-layout>
                    <v-flex>
                        <div class="home-team-members">
                            <h3>Home Team</h3>
                            <div v-for="(key, index) in homeTeamMembers">
                                {{ index + 1 }}. {{ key.firstName }} {{key.lastName }}
                            </div>
                        </div>

                    </v-flex>
                </v-layout>
            </v-container>
            <v-container>
                <v-layout>
                    <v-flex>
                        <div class="away-team-members">
                            <h3>Away Team</h3>
                            <div v-for="(key, index) in visitorTeamMembers">
                                {{ index + 1 }}. {{ key.firstName }} {{key.lastName }}
                            </div>
                        </div>

                    </v-flex>
                </v-layout>
            </v-container>

        </div>
    </div>
</template>

<script>
    export default {
        data: () => ({
            valid: false,
            fill: true,
            teamType: 'Team',
            homeTeamMembers: [],
            visitorTeamMembers: [],
            teamNameRules: [
                v => !!v || 'Team name is required',
                v => v.length <= 20 || 'Team name must be less than 20 characters',

            ],
            nameRules: [
                v => !!v || 'Name is required',
                v => v.length <= 15 || 'Name must be less than 15 characters',

            ]
        }),
        methods: {
            homeOrAway: function () {
                if (this.fill === true) {
                    this.teamType = 'Away'
                } else {
                    this.teamType = 'Home'
                }
            },
            getData: function (submitEvent) {
                this.homeTeam = submitEvent.target.elements.home.value;
                this.visitorTeam = submitEvent.target.elements.visitor.value;
            },
            getMemberData: function (submitEvent) {
                console.log(submitEvent.target.elements.teamType);
                // if (submitEvent.elements.teamType === 'Home') {
                //     this.homeTeamMembers.push({
                //         "firstName": submitEvent.target.elements.firstName.value,
                //         "lastName": submitEvent.target.elements.lastName.value,
                //         "teamType": this.teamType
                //     })
                // } else {
                //     this.visitorTeamMembers.push({
                //         "firstName": submitEvent.target.elements.firstName.value,
                //         "lastName": submitEvent.target.elements.lastName.value,
                //         "teamType": this.teamType
                //     })
                // }
            }
        }
    }
</script>

<style>

</style>