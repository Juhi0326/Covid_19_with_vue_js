<template>
  <div class="">
    <div></div>
    <div class="tablazat">
      <table class="table-striped">
        <thead>
          <tr>
            <th scope="col">Ország</th>
            <th scope="col">Esetek száma</th>
            <th scope="col">Felgyógyultak</th>
            <th scope="col">Elhunytak</th>
            <th scope="col">Mai esetek száma</th>
            <th scope="col">Mai elhunytak száma</th>
            <th scope="col">Aktív esetek</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(x, index) in adathalmaz.countryitems[0]"
            v-bind:key="index"
          >
            <td>{{ x.title }}</td>
            <td>{{ x.total_cases }}</td>
            <td>{{ x.total_recovered }}</td>
            <td>{{ x.total_deaths }}</td>
            <td>{{ x.total_new_cases_today }}</td>
            <td>{{ x.total_new_deaths_today }}</td>
            <td>{{ x.total_active_cases }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "../../../node_modules/axios";
export default {
  name: "Table",
  data() {
    return {
      list: [],
      adathalmaz: {}
    };
  },
  mounted() {
    axios
      .get("https://api.thevirustracker.com/free-api?countryTotals=ALL")
      .then(res => {
        this.list = [...this.list, res.data];
        console.log(this.list);
        this.adathalmaz = this.list[0];
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    sortByAllCases() {
      const sortable = Object.values(this.adathalmaz.countryitems[0]);
      this.list = sortable;
      console.log(this.list);
      this.list.sort((a, b) => b.total_cases - a.total_cases);
      console.log("kattintottam!");
    }
  },

  components: {}
};
</script>
<style scoped>
.tablazat {
  display: flex;
  justify-content: center;
}
</style>
