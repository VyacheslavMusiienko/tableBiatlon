<template>
  <div>
    <input type="search" v-model="search" placeholder="Filter By Name" />
    <table class="table">
      <thead>
        <tr>
          <th @click="sort('id')">#</th>
          <th @click="sort('name')">Имя</th>
          <th @click="sort('country')">Страна</th>
          <th @click="sort('scope')">Результат</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="table in filteredList" :key="table.id">
          <td>{{ table.id }}</td>
          <td>{{ table.name }}</td>
          <td>{{ table.country }}</td>
          <td>{{ table.scope }}</td>
        </tr>
      </tbody>
    </table>
    
  </div>
</template>

<script>
export default {
  name: "TheTable",
  data() {
    return {
      tables: [],
      search: "",
      sortBy: null,
      sortOrder: "asc",
    };
  },
  mounted() {
    this.fetchTodo();
  },
  methods: {
    async fetchTodo() {
      await this.axios
        .get("http://localhost:3004/result")
        .then((res) => {
          this.tables = res.data;
        })
        .catch((err) => {
          console.error(err);
        });
    },
    sort: function (sortBy) {
      if (this.sortBy === sortBy) {
        this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
      } else {
        this.sortBy = sortBy;
      }
    },
  },
  computed: {
    filteredList() {
      let text = this.search.toLowerCase();
      const sorted = this.tables.filter((e) => {
        return e.name.toLowerCase().indexOf(text) > -1;
      });
      return sorted.sort((a, b) => {
        let modifier = 1;
        if (this.sortOrder === "desc") modifier = -1;
        if (a[this.sortBy] < b[this.sortBy]) return -1 * modifier;
        if (a[this.sortBy] > b[this.sortBy]) return 1 * modifier;
        return 0;
      });
    },
  },
  watch: {
    filterOptions: function () {},
  },
};
</script>

<style scoped>
.table {
  width: 100%;
  border: none;
  margin-bottom: 20px;
}
.table thead th {
  font-weight: bold;
  text-align: left;
  border: none;
  padding: 10px 15px;
  background: #d8d8d8;
  font-size: 14px;
}
.table thead tr th:first-child {
  border-radius: 8px 0 0 8px;
}
.table thead tr th:last-child {
  border-radius: 0 8px 8px 0;
}
.table tbody td {
  text-align: left;
  border: none;
  padding: 10px 15px;
  font-size: 14px;
  vertical-align: top;
}
.table tbody tr:nth-child(even) {
  background: #f3f3f3;
}
.table tbody tr td:first-child {
  border-radius: 8px 0 0 8px;
}
.table tbody tr td:last-child {
  border-radius: 0 8px 8px 0;
}
.select {
  size: 25px;
  margin: 5px;
}
div {
  text-align: center;
}

</style>
