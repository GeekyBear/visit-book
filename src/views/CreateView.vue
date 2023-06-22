<template>
    <div class="container">
        <b-card bg-variant="light">
            <b-form-group :invalid-feedback="invalidFeedback" :state="state" label-cols-lg="3" label="Create visitor"
                label-size="lg" label-class="font-weight-bold pt-0" class="mb-0">
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
        <p v-if="errors.length">
            <b>Please correct the following error(s):</b>
        <ul>
            <li v-for="error in errors">{{ error }}</li>
        </ul>
        </p>
    </div>
</template>

<script>
export default {
    name: 'CreateView',
    data() {
        return {
            newVisitName: "",
            newVisitLastname: "",
            errors: [],
        }
    },
    computed: {
        state() {
            return this.newVisitName.length >= 3 && this.newVisitLastname.length >= 3
        },
        invalidFeedback() {
            if (this.newVisitName.length > 0 && this.newVisitName.length < 3) {
                return 'Enter a name that has at least 3 characters.'
            }

            if (this.newVisitLastname.length > 0 && this.newVisitLastname.length < 3) {
                return 'Enter a lastname that has at least 3 characters.'
            }
        }
    },
    methods: {
        async addVisit() {
            if (this.state) {
                let newVisit = {
                    firstname: this.newVisitName,
                    lastname: this.newVisitLastname,
                };

                await fetch(import.meta.env.VITE_API_URL + "/visitors", {
                    method: "post",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(newVisit)
                })

                this.newVisitName = "";
                this.newVisitLastname = "";
                this.$router.back();
            }
        },
        goBack() {
            console.log('Creation canceled');
            this.$router.back();
        },

    }
}
</script>

<style scoped>
.container {
    min-width: 600px
}
</style>