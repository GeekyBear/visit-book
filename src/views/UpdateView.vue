<template>
    <div>
        <div class="add-visit-wrapper">
            <p>ID: {{ $route.params.id }}</p>
            <p>Name</p>
            <input type="text" v-model="newVisitTitle" @keydown.enter="updateVisitor" />
            <p>Lastname</p>
            <input type="text" v-model="newVisitDescription" @keydown.enter="updateVisitor" />
        </div>
        <div class="add-visit-wrapper">
            <button @click="updateVisitor">Update visitor</button>
        </div>
    </div>
</template>
  
<script>
export default {
    name: 'UpdateView',
    mounted() {
        console.log(this.$route.params)
    },
    data() {
        return {
            newVisitTitle: "",
            newVisitDescription: "",
        }
    },
    async created() {
        const response = await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks/${this.$route.params.id}`);
        const resJson = await response.json();
        this.newVisitTitle = resJson.body.title;
        this.newVisitDescription = resJson.body.description;
    },
    components: {

    },
    methods: {
        async updateVisitor() {
            try {
                let newVisit = {
                    title: this.newVisitTitle,
                    description: this.newVisitDescription,
                    done: true
                };

                await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks/${this.$route.params.id}`, {
                    method: "put",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(newVisit)
                })


                this.newVisitTitle = "";
                this.newVisitDescription = "";
                this.$router.back()
            } catch (error) {
                console.log(error)
            }

        },
    }
}
</script>