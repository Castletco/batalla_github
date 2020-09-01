<template>
  <b-container>
    <b-row class="justify-content-center">
      <b-card
        title="Player 1"
        :img-src="valorImg"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="m-2"
      >
        <b-input-group>
          <template v-slot:prepend>
            <b-input-group-text >Username</b-input-group-text>
          </template>
          <b-form-input v-model="userName">{{userName}}</b-form-input>
          <b-input-group-append>
            <b-button size="sm" text="Button" variant="success" @click="updateResults">Go</b-button>
          </b-input-group-append>
        </b-input-group>
        <b-list-group v-if="info != null">
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

      <b-card
        title="Player 2"
        :img-src="valorImg2"
        img-alt="Image"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="m-2"
      >
        <b-input-group>
          <template v-slot:prepend>
            <b-input-group-text >Username</b-input-group-text>
          </template>
          <b-form-input v-model="userName2">{{userName2}}</b-form-input>
          <b-input-group-append>
            <b-button size="sm" text="Button" variant="success" @click="updateResults">Go</b-button>
          </b-input-group-append>
        </b-input-group>
        <b-list-group v-if="info2 != null">
          <b-list-group-item class="d-flex justify-content-between align-items-center">
            Public Repositories:
            <b-badge variant="primary" pill>{{info2.public_repos}}</b-badge>
          </b-list-group-item>
          <b-list-group-item class="d-flex justify-content-between align-items-center">
            Numbers of Followers:
            <b-badge variant="primary" pill>{{info2.followers}}</b-badge>
          </b-list-group-item>
          <b-list-group-item class="d-flex justify-content-between align-items-center">
            Numbers of Following:
            <b-badge variant="primary" pill>{{info2.following}}</b-badge>
          </b-list-group-item>
          <b-list-group-item class="d-flex justify-content-between align-items-center">
            Public Gists:
            <b-badge variant="primary" pill>{{info2.public_gists}}</b-badge>
          </b-list-group-item>
          <b-list-group-item class="d-flex justify-content-between align-items-center">
            Total Score:
            <b-badge variant="primary" pill>{{info2.public_repos + info2.followers + info2.following + info2.public_gists}}</b-badge>
          </b-list-group-item>
        </b-list-group>
      </b-card>
    </b-row>
    <button @click="battle">BATTLE!</button>
  </b-container>
</template>
<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      info: null,
      info2: null,
      userName: null,
      userName2: null,
      valorImg: null,
      valorImg2: null,
      totalscore1: 0,
      totalscore2: 0
      //isLoading: false    // it's not loading initially
    };
  },
  mounted() {
    this.getUserData();
  },
  methods: {
    updateResults() {
      this.getUserData();
    },
    async getUserData() {
      try {
        //this.isLoading = true;      // we're about to start the call, so that means we're loading
        let response = await this.$http.get(`https://api.github.com/users/${this.userName}`);  // instead of hardcoding a value, let's use whatever has been typed in the input box
        this.valorImg = response.data.avatar_url;
        this.info = response.data;
        this.totalscore1 = this.info.public_repos + this.info.followers + this.info.following + this.info.public_gists;
        let rs = await this.$http.get(`https://api.github.com/users/${this.userName2}`);
        this.valorImg2 = rs.data.avatar_url;
        this.info2 = rs.data;
        this.totalscore2 = this.info2.public_repos + this.info2.followers + this.info2.following + this.info2.public_gists;
      }
      catch(error) {
        console.log(`Something went wrong: ${error}`);
        //this.isLoading = false;     // we'll still want this regardless
        if (error.response) {
          // here, you may want to drill even further to handle 400 and 500 level errors differently
          console.log(error.response);
        }
        else if (error.request) {
          // this will only be reached if the request didn't ever receive a response
          console.log(error.request);
        }
        else {
          // something in the setup of the request triggered an error
          console.log(error.message);
        }
      }
    },
    battle() {
      if (this.totalscore1 > this.totalscore2) {
        //alert ("el ganador es: " + this.userName);
        this.$router.push({ name: 'Result', params: {ganador: this.userName}});
      } else {
        //alert ("el ganador es: " + this.userName2);
        this.$router.push({ name: 'Result', params: {ganador: this.userName2}});
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
