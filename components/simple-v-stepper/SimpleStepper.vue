<template>
    <div class="simple-stepper">
        <div class="stepper-progress-bar">
            <div v-for="(step, index) in stepsDescription"
                 v-bind:class="stepClass(index)">
                <div class="mdl_stepper-step-wrapper" @click.prevent="moveNextStep(index + 1)">
                    <div class="step-circle">
                        <span> {{index +1}} </span>
                    </div>
                    <div class="step-title">
                        {{step.name}}
                    </div>
                </div>
                <div v-if="index > 0" class="bar-left"></div>
                <div v-if="index + 1  < stepsDescription.length" class="bar-right"></div>
            </div>
        </div>

        <div class="step">
            <slot v-for="index in stepsDescription.length"
                  :name="'step-'+index"
                  v-if="stepNumber === index"
                  transition="fade">
                Slot for {{stepsDescription[index]}}
            </slot>
        </div>

        <div class="stepper-navigation">
            <button v-if="stepNumber > 1" class="button back" @click.prevent="moveToStep(stepNumber - 1)">
                back
            </button>
            <button v-else class="button back"> back</button>

            <button v-if="stepNumber < stepsDescription.length"
                    class="button next"
                    @click.prevent="moveNextStep(stepNumber + 1)">
                next
            </button>

            <button v-else class="button next" @click.prevent="finishStepper"> finish
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'SimpleStepper',
        props: {
            stepsDescription: [],
            stepValidation: {
                type: Boolean,
                default: false
            }
        },
        data: () => ({
            stepNumber: 1,
            defaultStepDescription: {
                name: 'Step Name',
                isRequired: true
            }
        }),
        methods: {
            moveToStep(nextStep) {
                this.stepNumber = nextStep
            },
            moveNextStep(nextStep) {
                if (this.stepValidation) {
                    this.$emit('tryEndStep', {
                        number: this.stepNumber,
                        move: () => {
                            this.moveToStep(nextStep)
                        }
                    })
                } else {
                    this.moveToStep(nextStep)
                }
            },
            resetStepper() {
                this.stepNumber = 1
            },
            stepClass(index) {
                return {
                    'step-navigation step-done': this.stepNumber > index + 1,
                    'step-navigation editable-step': this.stepNumber === index + 1,
                    'step-navigation': this.stepNumber < index + 1
                }
            },
            finishStepper() {
                if (this.stepValidation) {
                    this.$emit('tryEndStep', {
                        number: this.stepNumber,
                        move: () => {
                            this.$emit('onStepperFinish')
                        }
                    })
                } else {
                    this.$emit('onStepperFinish')
                }
            }
        }
    }
</script>

<style scoped>

    /*navigation button -- start*/
    .button {
        border: none;
        color: white;
        padding: 12px 28px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 4px 2px;
        -webkit-transition-duration: 0.4s; /* Safari */
        transition-duration: 0.4s;
        cursor: pointer;
    }

    .back {
        background-color: white;
        color: black;
        border: 2px solid #e7e7e7;
    }

    .back:hover {background-color: #e7e7e7;}

    .next {
        background-color: white;
        color: black;
        border: 2px solid #555555;
        float: right !important;
    }

    .next:hover {
        background-color: #555555;
        color: white;
    }

    .step {
        margin: auto;
        min-height: 500px;
        padding: 35px;
    }

    .stepper-navigation {
        padding: 0 35px 10px 35px;
    }

    .simple-stepper{
        margin: auto;
        width: 1100px;
    }

    /*progress bar*/
    .stepper-progress-bar {
        display: flex;
        justify-content: center;
        width: 100%;
        margin: 0 auto;
        /*padding: 24px;*/
    }

    .stepper-progress-bar .step-navigation {
        display: table-cell;
        position: relative;
        padding: 24px;
        width: 50%;
    }

    .stepper-progress-bar .step-navigation .step-title {
        margin-top: 16px;
        font-size: 20px;
        text-align: center;
        font-weight: 500;
        color: rgba(0, 0, 0, .87);
    }

    .stepper-progress-bar .mdl_stepper-step-wrapper:hover .step-circle,
    .stepper-progress-bar .mdl_stepper-step-wrapper:active .step-circle {
        background-color: black;
    }

    .stepper-progress-bar .step-circle {
        width: 50px;
        height: 50px;
        margin: 0 auto;
        background-color: #9E9E9E;
        box-shadow: 0 3px 6px rgba(0, 0, 0, .275);
        border-radius: 50%;
        text-align: center;
        line-height: 2em;
        font-size: 25px;
        color: white;
    }

    .stepper-progress-bar .step-circle > div {
        -webkit-transform: none;
        transform: none;
        -webkit-transition: -webkit-transform .175s cubic-bazier(.175, .67, .83, .67);
        transition: transform .175s cubic-bazier(.175, .67, .83, .67);
    }

    .stepper-progress-bar .editable-step .step-circle {
        background-color: black;
        -webkit-transform: scale(1.3, 1.3);
        transform: scale(1.3, 1.3);
        -webkit-animation: toggleBtnAnim .175s;
        animation: toggleBtnAnim .175s;
    }

    .stepper-progress-bar .editable-step .step-circle > div {
        -webkit-transform: rotate(45deg);
        transform: rotate(45deg);
        -webkit-transition: -webkit-transform .175s cubic-bazier(.175, .67, .83, .67);
        transition: transform .175s cubic-bazier(.175, .67, .83, .67);
    }

    .stepper-progress-bar .bar-left,
    .stepper-progress-bar .bar-right {
        position: absolute;
        top: 50px;
        height: 3px;
        border-top: 1px solid #BDBDBD;
    }

    .stepper-progress-bar .bar-left {
        left: 0;
        right: 50%;
        margin-right: 50px;
    }

    .stepper-progress-bar .bar-right {
        right: 0;
        left: 50%;
        margin-left: 50px;
    }

    .step-navigation.step-done .step-circle:before {
        content: "\2714";
    }

    .step-navigation.step-done .step-circle *,
    .step-navigation.editable-step .step-circle * {
        display: none;
    }

    .step-navigation.editable-step .step-circle:before {
        content: "\270E";
    }


    /*progress bar buttons animations*/
    @keyframes toggleBtnAnim {
        0% {
            -webkit-transform: scale(1, 1);
            transform: scale(1, 1);
        }
        25% {
            -webkit-transform: scale(1.4, 1.4);
            transform: scale(1.4, 1.4);
        }
        75% {
            -webkit-transform: scale(1.2, 1.2);
            transform: scale(1.2, 1.2);
        }
        100% {
            -webkit-transform: scale(1.3, 1.3);
            transform: scale(1.3, 1.3);
        }
    }

    @-webkit-keyframes toggleBtnAnim {
        0% {
            -webkit-transform: scale(1, 1);
            transform: scale(1, 1);
        }
        25% {
            -webkit-transform: scale(1.4, 1.4);
            transform: scale(1.4, 1.4);
        }
        75% {
            -webkit-transform: scale(1.2, 1.2);
            transform: scale(1.2, 1.2);
        }
        100% {
            -webkit-transform: scale(1.3, 1.3);
            transform: scale(1.3, 1.3);
        }
    }

</style>