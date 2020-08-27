<template>
  <div class="hello">
    <p>Look up by id: <input v-model="id" /><button @click="updateResults">Search</button></p>
    <p>The result of our request: {{ info.firmness }}</p>
    <p>The result of our request: {{ info.flavors }}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      info: null,
      id: 42,     // added this, setting 42 as the default value
    };
  },
  mounted() {
    this.getBerryData();
  },
  methods: {
    updateResults() {
      this.getBerryData();
    },
    async getBerryData() {
      try {
        let response = await this.$http.get(`https://pokeapi.co/api/v2/berry/${this.id}`);  // instead of hardcoding a value, let's use whatever has been typed in the input box
        this.info = response.data;
      }
      catch(error) {
        console.log(`Something went wrong: ${error}`);
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
