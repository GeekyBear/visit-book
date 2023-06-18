<template>
    <div class="container">
        <b-card bg-variant="light">
            <b-form-group label-cols-lg="3" label="Create visitor" label-size="lg" label-class="font-weight-bold pt-0"
                class="mb-0">
                <b-form-group label-cols="4" label-cols-lg="2" label="Name" label-for="input-default">
                    <b-form-input id="input-default" v-model="newVisitName" @keydown.enter="addVisit"></b-form-input>
                </b-form-group>
                <b-form-group label-cols="4" label-cols-lg="2" label="Lastname" label-for="input-default">
                    <b-form-input id="input-default" v-model="newVisitLastname" @keydown.enter="addVisit"></b-form-input>
                </b-form-group>
            </b-form-group>
            <b-button variant="outline-danger" class="mt-3 mx-2" @click="goBack">Cancel</b-button>
            <b-button variant="primary" class="mt-3" @click="addVisit">Create</b-button>
        </b-card>
    </div>
</template>

<script>
export default {
    name: 'CreateView',
    data() {
        return {
            newVisitName: "",
            newVisitLastname: "",
        }
    },
    components: {

    },
    methods: {
        async addVisit() {

            let newVisit = {
                firstname: this.newVisitName,
                lastname: this.newVisitLastname,
            };

            await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors", {
                method: "post",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(newVisit)
            })

            this.newVisitName = "";
            this.newVisitLastname = "";
            this.$router.back()
        },
        goBack() {
            console.log('Creation canceled');
            this.$router.back();
        }
    }
}
</script>

<style scoped>
.container {
    min-width: 600px
}
</style>