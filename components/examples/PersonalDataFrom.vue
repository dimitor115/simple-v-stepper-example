<template>

    <div class="example-form">
        <form>
            First name:<br>
            <input type="text" v-model="dataInstance['firstName']"><br><br>
            Last name:<br>
            <input type="text" v-model="dataInstance['lastName']"><br><br>
            Email :<br>
            <input type="text" v-model="dataInstance['email']"><br><br>
        </form>
        <form>
            Street:<br>
            <input type="text" v-model="dataInstance['street']"><br><br>
            City:<br>
            <input type="text" v-model="dataInstance['city']"><br><br>
            Country:<br>
            <input type="text" v-model="dataInstance['country']"><br><br>
        </form>
    </div>


</template>

<script>
    export default {
        name: "PersonalDataFrom",
        props: ['dataInstance']
    }
</script>

<style scoped>
    .example-form {
        display: flex;
    }

    .example-form > form {
        margin: 10px;
    }
</style>
