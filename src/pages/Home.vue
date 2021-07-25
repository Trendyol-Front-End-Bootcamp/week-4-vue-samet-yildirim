<template>
  <div>
    <Loading v-if="loading" />
    <div v-else>
      <div class="header">
        <input type="text" placeholder="Search by name" v-model="search" />
        <button @click="handleSearch">Search</button>
        <img class="logo" src="../assets/logo.png"/>
      </div>
      <div class="cardsWrapper">
        <StarshipCard
          v-for="(ship, index) in starships"
          :key="index"
          :ship="ship"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import StarshipCard from "../components/StarshipCard/StarshipCard.vue";
import Loading from "../components/Loading/Loading.vue";
export default {
  name: "Home",
  components: { StarshipCard, Loading },
  data() {
    return {
      loading: true,
      search: "",
      starships: []
    };
  },
  methods: {
    handleSearch: function() {
      this.loading = true;
      axios
        .get(`https://swapi.dev/api/starships?search=${this.search}`)
        .then(res => {
          this.starships = [...res.data.results];
          this.loading = false;
        });
    }
  },
  mounted() {
    this.loading = true;
    axios.get(`https://swapi.dev/api/starships`).then(res => {
      this.starships = [...res.data.results];
      this.loading = false;
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.cardsWrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  margin-top:25px;
}
.header{
  background:black;
  min-height:80px;
  display:flex;
  justify-content:center;
  align-items:center;
  position:relative;
}
.header input, .header button{
  padding:10px 20px;
  border-radius:5px;
}
.logo{
  position:absolute;
  height:70px;
  left:25px;
  top:50%;
  transform:translateY(-50%)
}
</style>
