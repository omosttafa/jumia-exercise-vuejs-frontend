<template>
  <div id="app" class="container">
    <label>Filter</label>
    <br />
    <select v-on:change="post" v-model="jumia.country">
      <option disabled value="">Select Country</option>
      <option v-for="country in countries" v-bind:key="country">
        {{ country }}
      </option>
    </select>

    <v-client-table
      v-if="tableSeen"
      :data="tableData"
      :columns="columns"
      :options="options"
    />

    <v-client-table
      v-if="miniTableSeen"
      :data="tableDataMini"
      :columns="columnsMini"
      :options="options"
    />
  </div>
</template>



<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);

export default {
  name: "CustomersList",
  data() {
    return {
      columns: ["id", "name", "phone", "validation", "countryCode"],
      columnsMini: ["id", "name", "phone"],
      tableData: [],
      tableDataMini: [],
      options: {
        headings: {
          id: "ID",
          name: "Name",
          phone: "Phone",
          validation: "Validation",
          countryCode: "Country Code",
        },
        filterable: false,
        perPageValues: [5, 10, 15, 20],
        sortable: ["id", "name", "phone", "validation", "countryCode"],
        // filterable: ["id","name", "phone","validation","countryCode"],
      },
      optionsMini: {
        headings: {
          id: "ID",
          name: "Name",
          phone: "Phone",
        },
        filterable: false,
        perPageValues: [5, 10, 15, 20],
        sortable: ["id", "name", "phone"],
        // filterable: ["id","name", "phone","validation","countryCode"],
      },
      jumia: {
        country: "",
      },
      tableSeen: false,
      miniTableSeen: true,
      countries: ["Cameron", "Ethiopia", "Morocco", "Mozambique", "Uganda"],
    };
  },
  mounted() {
    this.axios.get("http://localhost:8080/customers/").then((res) => {
      this.tableDataMini = res.data;
    });
  },
  methods: {
    post: function () {
      if (!this.tableSeen) {
        console.warn("test");
        this.tableSeen = !this.tableSeen;
        this.miniTableSeen = !this.miniTableSeen;
        this.tableDataMini= []
      }

      Vue.axios
        .get(
          "http://localhost:8080/customers/" + this.jumia.country.toLowerCase()
        )
        .then((resp) => {
          this.tableData = resp.data;
          console.warn(resp.data);
          console.warn(this.jumia.country);
        });
    },
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.VuePagination {
  text-align: center;
}

.vue-title {
  text-align: center;
  margin-bottom: 10px;
}

.vue-pagination-ad {
  text-align: center;
}

.glyphicon.glyphicon-eye-open {
  width: 16px;
  display: block;
  margin: 0 auto;
}

th:nth-child(3) {
  text-align: center;
}

.VueTables__child-row-toggler {
  width: 16px;
  height: 16px;
  line-height: 16px;
  display: block;
  margin: auto;
  text-align: center;
}

.VueTables__child-row-toggler--closed::before {
  content: "+";
}

.VueTables__child-row-toggler--open::before {
  content: "-";
}
</style>

