<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <table>
      <h2>Player 1</h2>
      <tr>
        <td>Username: </td>
        <td>{{ info.login }}</td>
      </tr>

      <tr>
        <td>Public repositories: </td>
        <td>{{ info.public_repos }}</td>
      </tr>

      <tr>
        <td>Number of followers: </td>
        <td>{{ info.followers }}</td>
      </tr>

      <tr>
        <td>Number Following: </td>
        <td>{{ info.following }}</td>
      </tr>

      <tr>
        <td>Public Gists: </td>
        <td>{{ info.public_gists }}</td>
      </tr>

      <tr>
        <td><strong>Total Score: </strong></td>
        <td><strong>{{ info.public_repos + info.followers + info.following + info.public_gists}}</strong></td>
      </tr>
    </table>
    <p>The result of our request: {{ info }}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      info: null
    }
  },
  mounted() {
    this.getBerryData();
  },
  methods: {
    async getBerryData() {
      try {
        let response = await this.$http.get("https://api.github.com/users/Castletco");
        this.info = response.data;      // a lot more information comes back in the response, so for now we only want the actual data of the response
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
table {
  align-content: center;
  horiz-align: center;
}
</style>
