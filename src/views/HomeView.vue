<template>
  <main>
    <h1>Building visit book</h1>
    <b-table striped hover :items="items" :fields="fields">
      <template v-slot:cell(edit)="data">
        <router-link :to="{
          name: 'UpdateView',
          params:
          {
            id: data.item.id
          }
        }" tag="button" class="btn btn-info"><b-icon icon="pencil-square"></b-icon></router-link>
      </template>
      <template v-slot:cell(delete)="data">
        <b-button size="sm" class="btn btn-danger" @click="removeVisit(data.item.id)">
          <b-icon icon="trash"></b-icon>
        </b-button>
      </template>
    </b-table>

    <div class="add-visit-wrapper">
      <button class="btn btn-primary" @click="addVisit">Add visit</button>
    </div>

  </main>
</template>
<script>
export default {
  name: 'Home',
  data() {
    return {
      newVisitTitle: "",
      newVisitDescription: "",
      items: [],
      fields: [
        { key: 'title' },
        { key: 'description' },
        { key: 'createdAt' },
        { key: 'Edit', label: 'Edit' },
        { key: 'Delete', label: 'Delete' }],
    }
  },
  async mounted() {
    try {
      const response = await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks");
      const resJson = await response.json();
      this.items = resJson.body.tasks;
    } catch (error) {
      console.log(error);
    }
  },
  components: {

  },
  methods: {
    async fetchItems() {
      const response = await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks");
      const resJson = await response.json();
      this.items = resJson.body.tasks;
    },
    async addVisit() {
      this.$router.push({ name: 'CreateView' })
    },
    async removeVisit(visitId) {
      await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/tasks/${visitId}`, {
        method: "delete",
        headers: {
          "Content-Type": "application/json"
        },
      });
      await this.fetchItems();
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0 auto;
}

button,
input {
  border-radius: 5px;
  padding: 5px 10px;
  border: 1px solid #aaa;
  margin: 5px;
}

.add-visit-wrapper {
  display: flex;
}

.add-visit-wrapper input {
  flex: 1
}
</style>