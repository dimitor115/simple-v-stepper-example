<template>
    <div>
        <div class="row stepper-progress-bar">
            <div class="simple-stepper">
                <div v-for="(step, index) in stepsDescription"
                     v-bind:class="stepClass(index)">
                    <div class="mdl_stepper-step-wrapper" @click.prevent="moveNextStep(index + 1)">
                        <div class="simple-stepper-circle">
                            <span> {{index +1}} </span>
                        </div>
                        <div class="simple-stepper-title">
                            {{step.name}}
                        </div>
                    </div>
                    <div v-if="index > 0" class="simple-stepper-bar-left"></div>
                    <div v-if="index + 1  < stepsDescription.length" class="simple-stepper-bar-right"></div>
                </div>
            </div>
        </div>

        <div class="row step">
            <slot v-for="index in stepsDescription.length"
                  :name="'step-'+index"
                  v-if="stepNumber === index"
                  transition="fade">
                Slot for {{stepsDescription[index]}}
            </slot>
        </div>

        <div class="navigation">
            <button v-if="stepNumber > 1" class="btn btn-default" @click.prevent="moveToStep(stepNumber - 1)">
                back
            </button>
            <button v-else class="btn btn-default disabled"> back</button>

            <button v-if="stepNumber < stepsDescription.length"
                    class="btn btn-primary pull-right"
                    @click.prevent="moveNextStep(stepNumber + 1)">
                next
            </button>

            <button v-else class="btn btn-success pull-right" @click.prevent="finishStepper"> finish
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'simple-stepper',
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
                    'simple-stepper-step step-done': this.stepNumber > index + 1,
                    'simple-stepper-step editable-step': this.stepNumber === index + 1,
                    'simple-stepper-step': this.stepNumber < index + 1
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

    .step {
        margin: 5px;
    }

    .navigation {
        margin-top: 15px;
    }

    .stepper-progress-bar {
        margin-bottom: 40px;
    }

    .stepper-progress-bar .simple-stepper {
        display: flex;
        width: 100%;
        margin: 0 auto;
    }

    .stepper-progress-bar .simple-stepper .simple-stepper-step{
        display: table-cell;
        position: relative;
        padding: 24px;
        width: 33%;
    }

    .stepper-progress-bar .simple-stepper .simple-stepper-step .simple-stepper-title {
        margin-top: 16px;
        font-size: 20px;
        text-align: center;
        font-weight: 500;
        color: rgba(0, 0, 0, .87);
    }

    .stepper-progress-bar .simple-stepper .mdl_stepper-step-wrapper:hover .simple-stepper-circle ,
    .stepper-progress-bar .simple-stepper .mdl_stepper-step-wrapper:active .simple-stepper-circle {
        background-color: black;
    }

    .stepper-progress-bar .simple-stepper .simple-stepper-circle {
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

    .stepper-progress-bar .simple-stepper .simple-stepper-circle > div {
        -webkit-transform: none;
        transform: none;
        -webkit-transition: -webkit-transform .175s cubic-bazier(.175, .67, .83, .67);
        transition: transform .175s cubic-bazier(.175, .67, .83, .67);
    }

    .stepper-progress-bar .simple-stepper .editable-step .simple-stepper-circle {
        background-color: $sbp-admin-fa-active-light;
        -webkit-transform: scale(1.3, 1.3);
        transform: scale(1.3, 1.3);
        -webkit-animation: toggleBtnAnim .175s;
        animation: toggleBtnAnim .175s;
    }

    .stepper-progress-bar .simple-stepper .editable-step .simple-stepper-circle > div {
        -webkit-transform: rotate(45deg);
        transform: rotate(45deg);
        -webkit-transition: -webkit-transform .175s cubic-bazier(.175, .67, .83, .67);
        transition: transform .175s cubic-bazier(.175, .67, .83, .67);
    }

    .stepper-progress-bar .simple-stepper .simple-stepper-bar-left,
    .stepper-progress-bar .simple-stepper .simple-stepper-bar-right {
        position: absolute;
        top: 50px;
        height: 3px;
        border-top: 1px solid #BDBDBD;
    }

    .stepper-progress-bar .simple-stepper .simple-stepper-bar-left {
        left: 0;
        right: 50%;
        margin-right: 50px;
    }
    .stepper-progress-bar .simple-stepper .simple-stepper-bar-right {
        right: 0;
        left: 50%;
        margin-left: 50px;
    }

    .simple-stepper .simple-stepper-step.step-done .simple-stepper-circle:before {
        content: "\2714";
    }

    .simple-stepper .simple-stepper-step.step-done .simple-stepper-circle *,
    .simple-stepper .simple-stepper-step.editable-step .simple-stepper-circle * {
        display: none;
    }

    .simple-stepper .simple-stepper-step.editable-step .simple-stepper-circle:before {
        content: "\270E";
    }

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