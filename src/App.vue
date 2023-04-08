<template>
  <div id="app">
    <h1 class="text-primary">Assignment</h1>
    <h2 class="text-primary">Covid Data represented in a tabular format</h2>
    <div class="container">
      <div class="row justify-content-between align-items-center">
        <div class="col-2">
          <b-dropdown id="dropdown-1" text="Show entries" class="m-md-2">
            <b-dropdown-item @click="chaneEntries">10</b-dropdown-item>
            <b-dropdown-item @click="chaneEntries">20</b-dropdown-item>
            <b-dropdown-item @click="chaneEntries">50</b-dropdown-item>
          </b-dropdown>
        </div>
        <div class="col-6">
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''"
                >Clear</b-button
              >
            </b-input-group-append>
          </b-input-group>
        </div>
      </div>

      <b-table
        striped
        hover
        bordered
        :per-page="perPage"
        :current-page="currentPage"
        :items="covid_data"
        fixed
        sort-icon-left
        :fields="table_fields"
        :filter="filter"
        :tbody-transition-props="transProps"
      ></b-table>

      <div class="row justify-content-center">
        <div class="col-12">
          <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
          ></b-pagination>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      tableDropDown: null,
      currentPage: 1,
      perPage: 10,
      rows: null,
      covid_data: [],
      filter: null,
      transProps: {
        name: "flip-list",
      },
      table_fields: [
        {
          key: "date",
          sortable: true,
        },
        {
          key: "key",
          sortable: true,
        },
        {
          key: "new_confirmed",
          sortable: true,
        },
        {
          key: "new_deceased",
          sortable: true,
        },
        {
          key: "total_confirmed",
          sortable: true,
        },
        {
          key: "total_deceased",
          sortable: true,
        },
      ],
      showEntries: "",
    };
  },
  async mounted() {
    const api_response = await this.$http.get(
      "http://localhost:3001/api/getAll"
    );
    console.log(api_response);
    api_response.data.data.forEach((_d) => {
      var _filtered_response = {};
      _filtered_response.date = _d["date"];
      _filtered_response.key = _d["key"];
      _filtered_response.new_confirmed = _d["new_confirmed"];
      _filtered_response.new_deceased = _d["new_deceased"];
      _filtered_response.total_confirmed = _d["total_confirmed"];
      _filtered_response.total_deceased = _d["total_deceased"];
      this.covid_data.push(_filtered_response);
    });
    this.rows = api_response.data.total;

    // console.log(this.covid_data);
  },
  methods: {
    chaneEntries(event) {
      this.perPage = event.target.innerText;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
