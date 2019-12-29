<template>
  <div class="overflow-auto">
    <b-table
      id="cat-table"
      borderless
      striped
      :items="items"
      :fields="fields"
      :per-page="perPage"
      :current-page="currentPage"
      small
      :busy.sync="isBusy"
    >
      <template v-slot:table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>

      <template v-slot:cell(user.name)="data">
        {{data.value.first}} {{data.value.last}}
      </template>
    </b-table>

    <b-pagination
      class="cat-pagination"
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      align="fill"
      first-text="Primero"
      prev-text="Anterior"
      next-text="Siguiente"
      last-text="Ultimo"
      aria-controls="cat-table"
    ></b-pagination>
  </div>
</template>

<script>
import Axios from 'axios';

export default {
  data() {
    return {
      isBusy: false,
      perPage: 10,
      currentPage: 1,
      fields: [{ key: 'user.name' }, 'text', 'upvotes'],
      items: []
    };
  },
  mounted() {
    this.isBusy = true;
    this.getInfo()
      .then(response => {
        //console.log('response: ', response);
        this.isBusy = false;
      })
      .catch(error => {
        console.error(error);
      });
  },
  methods: {
    async getInfo() {
      const response = await Axios.get(
        'https://cors-anywhere.herokuapp.com/https://cat-fact.herokuapp.com/facts'
      );
      this.items = await response.data.all;
      return this.items;
    }
  },
  computed: {
    rows() {
      //console.log('length: ', this.items.length);
      return this.items.length;
    },
    setPages() {
      const numberOfPages = Math.ceil(this.todo.length / this.perPage);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#cat-table {
  font-weight: bold;
}
.cat-pagination {
  position: fixed;
  bottom: 0;
  width: 100%;
  box-shadow: black 1px 0px 5px;
}
</style>
