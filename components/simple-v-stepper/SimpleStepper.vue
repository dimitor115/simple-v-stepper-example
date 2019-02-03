<template>
    <section class="simple-stepper">
        <header class="progress-bar">
            <div v-for="(step, index) in stepsDescription"
                 v-bind:class="stepClass(index)">
                <button class="step-navigation-button" @click.prevent="moveNextStep(index + 1)">
                    <div class="step-navigation-icon">
                        <span> {{index +1}} </span>
                    </div>
                    <span class="step-navigation-title">
                        {{step.name}}
                    </span>
                </button>
                <!-- <div v-if="index > 0" class="bar-left"></div> -->
                <div v-if="index + 1  < stepsDescription.length" class="step-navigation-deco"></div>
            </div>
        </header>

        <section class="step-wrapper">
            <slot v-for="index in stepsDescription.length"
                  :name="'step-'+index"
                  v-if="stepNumber === index"
                  transition="fade">
                Slot for step {{index}}
            </slot>
        </section>

        <footer class="footer-navigation">
            <button @click.prevent="resetStepper" class="footer-button footer-button--reset">Reset</button>

            <button class="footer-button" @click.prevent="moveToStep(stepNumber - 1)" :disabled="stepNumber <= 1">
                Back
            </button>

            <button v-if="stepNumber < stepsDescription.length"
                    class="footer-button footer-button--next"
                    @click.prevent="moveNextStep(stepNumber + 1)">
                Next
            </button>
            <button v-else class="footer-button footer-button--next" @click.prevent="finishStepper">
                Finish
            </button>
        </footer>
    </section>
</template>

<script>
    export default {
        name: 'SimpleStepper',
        props: {
            stepsDescription: {
                type: Array,
                default: []
            },
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
                this.$emit('onReset')
            },
            stepClass(index) {
                return {
                    'step-navigation step-navigation--done': this.stepNumber > index + 1,
                    'step-navigation step-navigation--editable': this.stepNumber === index + 1,
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
    * {
        box-sizing: border-box;
    }

    button {
        font: inherit;
        appearance: none;
        border: none;
        padding: 0;
        margin: 0;
        background: transparent;
        cursor: pointer;
    }

    .simple-stepper {
        background: #fff;
        width: 100%;
        border-radius: 16px;
    }

    .progress-bar {
        display: flex;
        justify-content: center;
        width: 100%;
        padding: 20px 10px;
    }

    .step-navigation {
        position: relative;
        flex: 1 0 0;
    }

    .step-navigation-button {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
        outline: none;
        color: #444;
    }

    .step-navigation-icon {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 48px;
        height: 48px;
        border-radius: 50%;
        border: 2px solid #eee;
        font-size: 15px;
        transition: all .1s ease-in-out;
    }

    .step-navigation-title {
        margin-top: 10px;
        font-weight: 500;
        color: #666;
        font-size: 15px;
    }

    .step-navigation-button:hover .step-navigation-icon,
    .step-navigation-button:focus .step-navigation-icon {
        background: #0D47A1;
        color: #fff;
        border-color: transparent;
    }
    
    .step-navigation-deco {
        width: 36%;
        height: 2px;
        background: #eee;
        position: absolute;
        right: -18%;
        top: 25px;
    }

    .step-navigation--editable .step-navigation-icon {
        background: #0D47A1;
        color: #fff;
        border-color: transparent;
        transform: scale(1.2);
        animation: toggleBtnAnim .175s;
    }

    .step-navigation--done .step-navigation-icon span,
    .step-navigation--editable .step-navigation-icon span {
        display: none;
    }

    .step-navigation--done .step-navigation-icon:before {
        content: "\2714";
    }
    
    .step-navigation--done .step-navigation-icon {
        background: #eee;    
    }
    
    .step-navigation--editable .step-navigation-icon:before {
        content: "\270E";
    }

    .step-wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 20px 10px;
    }

    .footer-navigation {
        display: flex;
        justify-content: flex-end;
        padding: 20px;
        background: #eee;
        border-bottom-left-radius: inherit;
        border-bottom-right-radius: inherit;
    }

    .footer-button {
        padding: 8px 16px;
        font-size: 14px;
        border-radius: 8px;
        color: #444;
        font-weight: 500;
        outline: none;
        transition: background .1s ease-in-out;
    }

    .footer-button:hover,
    .footer-button:focus {
        background: #ddd;
    }

    .footer-button:active {
        background: #ccc;
    }

    .footer-button--reset {
        margin-right: auto;
    }

    .footer-button--next {
        background: #0D47A1;
        color: #eee;
        letter-spacing: .3px;
        margin-left: 5px;
    }

    .footer-button--next:hover,
    .footer-button--next:focus {
        background: #003483;
    }

    .footer-button--next:active {
        background: #03265a;
    }


    @keyframes toggleBtnAnim {
        0% {
            transform: scale(1, 1);
        }
        25% {
            transform: scale(1.5, 1.5);
        }
        75% {
            transform: scale(1.1, 1.1);
        }
        100% {
            transform: scale(1.2, 1.2);
        }
    }

</style>
