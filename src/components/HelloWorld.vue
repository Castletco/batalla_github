<template>
  <div class="hello">
    <b-card
      title="Player 1"
      :img-src="valorImg"
      img-alt="Image"
      img-top
      tag="article"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <b-input-group>
        <template v-slot:prepend>
          <b-input-group-text >Username</b-input-group-text>
        </template>
        <b-form-input v-model="userName"></b-form-input>
        <b-input-group-append>
          <b-button size="sm" text="Button" variant="success" @click="updateResults">Go</b-button>
        </b-input-group-append>
      </b-input-group>
      <b-list-group>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          Public Repositories:
          <b-badge variant="primary" pill>{{info.public_repos}}</b-badge>
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          Numbers of Followers:
          <b-badge variant="primary" pill>{{info.followers}}</b-badge>
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          Numbers of Following:
          <b-badge variant="primary" pill>{{info.following}}</b-badge>
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          Public Gists:
          <b-badge variant="primary" pill>{{info.public_gists}}</b-badge>
        </b-list-group-item>
        <b-list-group-item class="d-flex justify-content-between align-items-center">
          Total Score:
          <b-badge variant="primary" pill>{{info.public_repos + info.followers + info.following + info.public_gists}}</b-badge>
        </b-list-group-item>
      </b-list-group>
    </b-card>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      info: null,
      userName: null,
      valorImg: null,
    };
  },
  mounted() {
    this.getUserData();
  },
  methods: {
    updateResults() {
      console.log("username:" + this.userName)
      this.getUserData();
    },
    async getUserData() {
      try {
        let response = await this.$http.get(`https://api.github.com/users/${this.userName}`);  // instead of hardcoding a value, let's use whatever has been typed in the input box
        this.valorImg = response.data.avatar_url
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
