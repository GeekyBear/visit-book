<template>
  <main class="container">
    <b-table class="btable" stacked="sm" responsive="md" striped hover :items="items" :fields="fields">
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
        <b-button class="btn btn-danger" @click="removeVisit(data.item.id)">
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

      items: [],
      fields: [
        { key: 'firstname' },
        { key: 'lastname' },
        { key: 'createdAt' },
        { key: 'Edit', label: 'Edit' },
        { key: 'Delete', label: 'Delete' }],
    }
  },
  async mounted() {
    try {
      const response = await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors");
      const resJson = await response.json();
      this.items = resJson.body.visitors;

    } catch (error) {
      console.log(error);
    }
  },
  components: {

  },
  methods: {
    async fetchItems() {
      const response = await fetch("https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors");
      const resJson = await response.json();
      this.items = resJson.body.visitors;
    },

    async addVisit() {
      this.$router.push({ name: 'CreateView' })
    },

    async removeVisit(visitId) {
      this.$bvModal.msgBoxConfirm('Please confirm that you want to delete everything.', {
        title: 'Please Confirm',
        size: 'sm',
        buttonSize: 'sm',
        okVariant: 'danger',
        okTitle: 'YES',
        cancelTitle: 'NO',
        footerClass: 'p-2',
        hideHeaderClose: false,
        centered: true
      })
        .then(async (value) => {
          if (value) {
            await fetch(`https://0slrsqlw38.execute-api.sa-east-1.amazonaws.com/visitors/${visitId}`, {
              method: "delete",
              headers: {
                "Content-Type": "application/json"
              },
            });
            await this.fetchItems();
          }
        })
        .catch(err => {
          console.log(err);
        })
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
  justify-content: flex-end;
}

.add-visit-wrapper input {
  flex: 1;
}


@media (max-width: 1024px) {

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }
}
</style>