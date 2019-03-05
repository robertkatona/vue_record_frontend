<template>
    <div class="app">
        <div class="text-xs-center">
            <v-btn outline color="red">Start</v-btn>
            <v-form @submit.prevent="getTeamNames" v-model="valid">
                <v-container fill-height>
                    <v-layout>
                        <TeamName name="home" label="Home Team"></TeamName>
                        <TeamName name="visitor" label="Visitor Team"></TeamName>
                    </v-layout>
                </v-container>
                <v-btn type="submit" outline color="green">Submit</v-btn>
            </v-form>
            <div class="home-team-table" style="width: 500px; height: 500px; margin: auto">
                <TeamEditor></TeamEditor>
            </div>
            <div class="visitor-team-table" style="width: 500px; height: 500px; margin: auto">
                <TeamEditor></TeamEditor>
            </div>
        </div>
    </div>
</template>

<script>

    import TeamEditor from "./components/TeamEditor.vue";
    import TeamName from "./components/TeamName.vue";


    export default {
        components: {
            TeamEditor,
            TeamName
        },
        data: () => ({
            teamName: '',
            homeTeam: '',
            visitorTeam: '',
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
                {text: '#Number', value: 'number'}
            ],
            editedIndex: -1,
            editedPlayer: {
                firstName: '',
                lastName: '',
                number: '',
                team: ''
            },
            defaultIPlayer: {
                firstName: '',
                lastName: '',
                number: '',
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
            getTeamNames: function (submitEvent) {
                this.homeTeam = submitEvent.target.elements.home.value;
                this.visitorTeam = submitEvent.target.elements.visitor.value;
                console.log('Home: ' + this.homeTeam);
                console.log('Visitor: ' + this.visitorTeam);
            }
        }
    }
</script>

<style>

</style>