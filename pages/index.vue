<template>
    <div class="example">
        <simple-stepper class="example-stepper" :steps-description="stepsDescriptions">
            <div slot="step-1">
                <h1> soidfjsodfjsdif </h1>
            </div>
            <div slot="step-2">
                <h1> sdasdasdasd </h1>
            </div>
        </simple-stepper>
    </div>
</template>

<script>
    import SimpleStepper from "../components/simple-v-stepper/SimpleStepper";

    export default {
        name: "index",
        components: {SimpleStepper},
        data: () => ({
            stepsDescriptions: [
                {name: "Bla bla"},
                {name: "HEjo"},
                {name: "HEjo"}
            ]
        })
    }

</script>

<style scoped>
    .example-stepper {
        background-color: #f7f8fb;
    }
</style>