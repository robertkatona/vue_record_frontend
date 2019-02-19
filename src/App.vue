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
                                    :rules="nameRules"
                                    :counter="20"
                                    label="Home team"
                                    required
                            ></v-text-field>
                        </v-flex>
                        <v-flex xs6>
                            <v-text-field
                                    name="visitor"
                                    v-model="visitorTeam"
                                    :rules="nameRules"
                                    :counter="20"
                                    label="Visitor Team"
                                    required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>
                </v-container>
                <v-btn type="submit" outline color="green">Submit</v-btn>
            </v-form>
            <!--<div class="text-xs-center">-->
                <v-form class="addMember" @submit.prevent="getMemberData">
                    <v-container fill-height>
                        <!--<v-layout align-center justify-center>-->
                        <v-layout>
                            <v-flex xs6>
                                <v-text-field
                                        name="firstName"
                                        v-model="firstName"
                                        :rules="nameRules"
                                        :counter="20"
                                        label="First Name"
                                        required
                                ></v-text-field>
                            </v-flex>
                            <v-flex xs6>
                                <v-text-field
                                        name="lastName"
                                        v-model="lastName"
                                        :rules="nameRules"
                                        :counter="20"
                                        label="Last Name"
                                        required
                                ></v-text-field>
                            </v-flex>
                            <v-flex xs2>
                                <v-switch v-model="fill" label="Team" v-bind="homeOrAway()"></v-switch>
                            </v-flex>
                            <v-flex xs3>
                                <v-btn @submit.prevent="getMemberData" type="submit" outline color="blue">Add member</v-btn>
                            </v-flex>
                        </v-layout>
                    </v-container>
                </v-form>
            <!--</div>-->
        </div>
    </div>
</template>

<script>
    export default {
        data: () => ({
            valid: false,
            fill: true,
            teamType: 'Team',
            homeTeam: '',
            homeTeamMembers: [],
            visitorTeam: '',
            visitorTeamMembers: [],
            nameRules: [
                v => !!v || 'Team name is required',
                v => v.length <= 20 || 'Team name must be less than 20 characters',

            ]
        }),
        methods: {
            homeOrAway: function () {
                if (this.fill === true) {
                    this.teamType = 'Away'
                } else {
                    this.teamType = 'Home'
                }
                console.log(this.teamType)
            },
            getData: function (submitEvent) {
                this.homeTeam = submitEvent.target.elements.home.value;
                this.visitorTeam = submitEvent.target.elements.visitor.value;
                console.log('Home:' + this.homeTeam);
                console.log('Visitor:' + this.visitorTeam);
            },
            getMemberData: function (submitEvent) {
                this.homeTeamMembers.push({
                    "firstName": submitEvent.target.elements.firstName.value,
                    "lastName": submitEvent.target.elements.lastName.value,
                    "teamType": this.teamType
                });
                console.log(this.homeTeamMembers);
            }
        }
    }
</script>

<style>

</style>