<template>
    <div class="container">
        <b-card bg-variant="light">
            <b-form-group :invalid-feedback="invalidFeedback" :state="state" label-cols-lg="3" label="Update visitor"
                label-size="lg" label-class="font-weight-bold pt-0" class="mb-0">
                <b-form-group label-cols="4" label-cols-lg="2" label="Name" label-for="input-default">
                    <b-form-input id="input-default" v-model="newVisitName" @keydown.enter="updateVisitor"></b-form-input>
                </b-form-group>
                <b-form-group label-cols="4" label-cols-lg="2" label="Lastname" label-for="input-default">
                    <b-form-input id="input-default" v-model="newVisitLastname"
                        @keydown.enter="updateVisitor"></b-form-input>
                </b-form-group>
            </b-form-group>
            <b-button variant="outline-danger" class="mt-3 mx-2" @click="goBack">Cancel</b-button>
            <b-button variant="primary" class="mt-3" @click="updateVisitor">Update</b-button>
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
    name: 'UpdateView',
    data() {
        return {
            newVisitName: "",
            newVisitLastname: "",
            errors: [],
        }
    },
    async created() {
        const response = await fetch(import.meta.env.VITE_API_URL + `/visitors/${this.$route.params.id}`);
        const resJson = await response.json();
        this.newVisitName = resJson.body.firstname;
        this.newVisitLastname = resJson.body.lastname;
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
        async updateVisitor() {
            if (this.state) {
                try {
                    let newVisit = {
                        firstname: this.newVisitName,
                        lastname: this.newVisitLastname,
                    };

                    const response = await fetch(import.meta.env.VITE_API_URL + `/visitors/${this.$route.params.id}`, {
                        method: "put",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify(newVisit)
                    })

                    this.newVisitName = "";
                    this.newVisitLastname = "";
                    this.$router.back();
                } catch (error) {
                    console.log(error)
                }
            }
        },
        goBack() {
            console.log('Update canceled');
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