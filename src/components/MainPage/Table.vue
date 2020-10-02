<template>
  <div class="">
    <div></div>
    <div class="tablazat">
      <table class="table-striped">
        <thead>
          <tr>
            <th scope="col" @click="sortByCountries()">Ország</th>
            <th scope="col" @click="sortByAllCases()">Esetek száma</th>
            <th scope="col" @click="sortByRecovered()">Felgyógyultak</th>
            <th scope="col" @click="sortByDeaths()">Elhunytak</th>
            <th scope="col">Mai esetek száma</th>
            <th scope="col">Mai elhunytak száma</th>
            <th scope="col">Aktív esetek</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(x, index) in list" v-bind:key="index">
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
      adathalmaz: {},
      asc: false,
      ascCountry: false,
      ascRecovered: false,
      ascDeaths: false
    };
  },
  mounted() {
    axios
      .get("https://api.thevirustracker.com/free-api?countryTotals=ALL")
      .then(res => {
        this.list = [...this.list, res.data];
        this.adathalmaz = this.list[0];
        this.list = Object.values(this.adathalmaz.countryitems[0]);
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    sortByAllCases() {
      if (this.asc === true) {
        this.list.sort((a, b) => b.total_cases - a.total_cases);
        this.asc = false;
      } else {
        this.list.sort((a, b) => a.total_cases - b.total_cases);
        this.asc = true;
      }
    },
    sortByCountries() {
      console.log(this.list);
      if (this.ascCountry === true) {
        this.list.sort((a, b) =>
          a.title > b.title ? 1 : b.title > a.title ? -1 : 0
        );
        this.ascCountry = false;
        console.log(this.list);
      } else {
        this.list.sort((a, b) =>
          b.title > a.title ? 1 : a.title > b.title ? -1 : 0
        );
        this.ascCountry = true;
        console.log(this.list);
      }
    },
    sortByRecovered() {
      if (this.ascRecovered === true) {
        this.list.sort((a, b) => b.total_recovered - a.total_recovered);
        this.ascRecovered = false;
      } else {
        this.list.sort((a, b) => a.total_recovered - b.total_recovered);
        this.ascRecovered = true;
      }
    },
    sortByDeaths() {
      if (this.ascDeaths === true) {
        this.list.sort((a, b) => b.total_deaths - a.total_deaths);
        this.ascDeaths = false;
      } else {
        this.list.sort((a, b) => a.total_deaths - b.total_deaths);
        this.ascDeaths = true;
      }
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
