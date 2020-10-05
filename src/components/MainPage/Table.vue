<template>
  <div class="col">
    <h1 class="col">Szűrés ország szerint</h1>
    <input class="col-5" type="text" placeholder="Keresés" v-model="nameFilter" />
    <br/>
    <br/>
    <div class="col">
      <table class="table-striped col">
        <thead>
          <tr>
            <th scope="col" @click="sortByCountries()">Ország</th>
            <th scope="col" @click="sortByAllCases()">Esetek száma</th>
            <th scope="col" @click="sortByRecovered()">Felgyógyultak</th>
            <th scope="col" @click="sortByDeaths()">Elhunytak</th>
            <th scope="col" @click="sortByDailyCases()">Mai esetek száma</th>
            <th scope="col" @click="sortByDailyDeaths()">
              Mai elhunytak száma
            </th>
            <th scope="col" @click="sortByDailyActiveCases()">Aktív esetek</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(x, index) in FilterCounty" v-bind:key="index">
            <td>{{ x.title }}</td>
            <td>{{ x.total_cases | numbers }}</td>
            <td>{{ x.total_recovered | numbers }}</td>
            <td>{{ x.total_deaths | numbers }}</td>
            <td>{{ x.total_new_cases_today | numbers }}</td>
            <td>{{ x.total_new_deaths_today | numbers }}</td>
            <td>{{ x.total_active_cases | numbers }}</td>
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
      nameFilter: "",
      list: [],
      szurtLista: [],
      adathalmaz: {},
      asc: false,
      ascCountry: false,
      ascRecovered: false,
      ascDeaths: false,
      ascDailyCases: false,
      ascDailyDeaths: false,
      ascActiveCases: false
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
    },
    sortByDailyCases() {
      if (this.ascDailyCases === true) {
        this.list.sort(
          (a, b) => b.total_new_cases_today - a.total_new_cases_today
        );
        this.ascDailyCases = false;
      } else {
        this.list.sort(
          (a, b) => a.total_new_cases_today - b.total_new_cases_today
        );
        this.ascDailyCases = true;
      }
    },
    sortByDailyDeaths() {
      if (this.ascDailyDeaths === true) {
        this.list.sort(
          (a, b) => b.total_new_deaths_today - a.total_new_deaths_today
        );
        this.ascDailyDeaths = false;
      } else {
        this.list.sort(
          (a, b) => a.total_new_deaths_today - b.total_new_deaths_today
        );
        this.ascDailyDeaths = true;
      }
    },
    sortByDailyActiveCases() {
      if (this.ascActiveCases === true) {
        this.list.sort((a, b) => b.total_active_cases - a.total_active_cases);
        this.ascActiveCases = false;
      } else {
        this.list.sort((a, b) => a.total_active_cases - b.total_active_cases);
        this.ascActiveCases = true;
      }
    }
  },

  computed: {
    FilterCounty() {
      return this.list.filter(item => {
        return JSON.stringify(item).includes(this.nameFilter);
      });
    }
  },
  filters: {
    capitalize: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    uppercase: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.toUpperCase();
    },
    numbers: function(value) {
      return parseFloat(value).toLocaleString("hu");
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
/* input {
  width: 100%;
} */
</style>
