<template>
    <div class="example-wrapper">
        <simple-stepper class="example" :steps-description="stepsDescriptions" @onReset="resetData">
            <personal-data-from 
                class="step"
                slot="step-1"
                ref="personalData"
                :data-instance="personalDataInstance" />
            <photo-upload 
                class="step" slot="step-2" :picture-instance="avatarInstance" />
            <genre-choose 
                class="step" :genres="exampleGenres"  slot="step-3" />
      </simple-stepper>
    </div>
</template>

<script>
    import SimpleStepper from "../components/simple-v-stepper/SimpleStepper";
    import PersonalDataFrom from "../components/examples/PersonalDataFrom";
    import PhotoUpload from "../components/examples/PhotoUpload";
    import GenreChoose from "../components/examples/GenreChoose";

    export default {
        name: "index",
        components: {GenreChoose, PhotoUpload, PersonalDataFrom, SimpleStepper},
        data: () => ({
            stepsDescriptions: [
                {name: "Personal Data"},
                {name: "Avatar"},
                {name: "Preferences"}
            ],
            personalDataInstance: {
                firstName: null,
                lastName: null,
                email: null,
                street: null,
                city: null,
                country: null
            },
            avatarInstance: {
                value: ""
            },
            exampleGenres : {
                action : false,
                adventure: false,
                comedy: false,
                crime: false,
                drama: false,
                fantasy: false,
                horror: false,
                political: false
            }
        }),
        methods: {
            //TODO: make it more elegant
            resetData() {
                this.personalDataInstance = {
                    firstName: null,
                        lastName: null,
                        email: null,
                        street: null,
                        city: null,
                        country: null
                },
                this.avatarInstance = {
                    value: ""
                },
                this.exampleGenres = {
                    action : false,
                        adventure: false,
                        comedy: false,
                        crime: false,
                        drama: false,
                        fantasy: false,
                        horror: false,
                        political: false
                }
            }
        }
    }

</script>

<style>
    body {
        margin: 0;
        background: #333;
        font-family: 'Roboto';
        background: url(https://images.unsplash.com/photo-1545171653-1e90329b6600?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1920&q=80);
        background-size: cover;
    }

    .example-wrapper {
        height: 100vh;
        max-width: 600px;
        display: flex;
        align-items: center;
        margin: 0 auto;
    }

    .example {
        box-shadow: 0 3px 12px 0 rgba(0,0,0,0.12); 
    }

    .example .step {
        min-height: 300px;
    }

</style>
