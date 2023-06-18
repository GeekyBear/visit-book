<template>
    <div class="container">
        <b-card bg-variant="light">
            <b-form-group label-cols-lg="3" label="Update visitor" label-size="lg" label-class="font-weight-bold pt-0"
                class="mb-0">
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
    </div>
</template>
  
<script>
export default {
    name: 'UpdateView',
    data() {
        return {
            newVisitName: "",
            newVisitLastname: "",
        }
    },
    async created() {
        const response = await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors/${this.$route.params.id}`);
        const resJson = await response.json();
        this.newVisitName = resJson.body.firstname;
        this.newVisitLastname = resJson.body.lastname;
    },
    components: {

    },
    methods: {
        async updateVisitor() {
            try {
                let newVisit = {
                    firstname: this.newVisitName,
                    lastname: this.newVisitLastname,
                };

                const response = await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors/${this.$route.params.id}`, {
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