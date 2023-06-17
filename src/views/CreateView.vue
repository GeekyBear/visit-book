<template>
    <div>
        <div class="add-visit-wrapper">
            <p>Name</p>
            <input type="text" v-model="newVisitTitle" />
            <p>Lastname</p>
            <input type="text" v-model="newVisitDescription" @keydown.enter="addVisit" />
        </div>
        <div class="add-visit-wrapper">
            <button @click="addVisit">Create visitor</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CreateView',
    data() {
        return {
            newVisitTitle: "",
            newVisitDescription: "",
        }
    },
    components: {

    },
    methods: {
        async addVisit() {

            let newVisit = {
                title: this.newVisitTitle,
                description: this.newVisitDescription,
            };

            await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks", {
                method: "post",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(newVisit)
            })

            this.newVisitTitle = "";
            this.newVisitDescription = "";
            this.$router.back()
        },
    }
}
</script>