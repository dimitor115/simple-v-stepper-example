<template>
    <div class="example">
        <simple-stepper class="example-stepper" :steps-description="stepsDescriptions" @onReset="resetData">
            <div slot="step-1">
                <personal-data-from
                        ref="personalData"
                        :data-instance="personalDataInstance">
                </personal-data-from>
            </div>
            <div slot="step-2">
                <photo-upload
                        :picture-instance="avatarInstance">
                </photo-upload>
            </div>

            <div slot="step-3">
                <genre-choose :genres="exampleGenres">
                </genre-choose>
            </div>
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

<style scoped>
    .example{
        height: 2000px;
        padding-top: 100px;
        background-color: #F5F5F5;
    }

    .example-stepper {
        /*background-color: #f7f8fb;*/
        width: 1000px;
        box-shadow: 5px 10px 18px #888888;
    }
</style>
