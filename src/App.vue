<template>
  <div>
    <Navbar />
    <div class="container grid-xs py-2">

      <form>
        <h1>Github Search</h1>
        <div class="input-group">
          <input type="text" @keyup="getUser" class="form-input" placeholder="Informe um usuário" required/>
          <button class="btn btn-primary input-group-btn">
            <i class="icon icon-search"></i> Buscar
            </button>
          
        </div>
      </form>
      <hr>
      

      <div class="row mt-3" v-if="user.length !== 0">
        <div class="col-md-4">
          <Profile :user="user" />
        </div>
        <div class="col-md-8"><Repos v-for="repo in repos" :repo="repo" :key="repo"/>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import Navbar from "./components/Navbar.vue";
import Profile from "./components/Profile.vue";
import Repos from "./components/Repos.vue";

export default {
  name: "app",
  data() {
    return {
      github: {
        url: "https://api.github.com/users",
        client_id: "Iv1.d3173fe62161d289",
        client_secret: "e2b2f6fd48cdcd796c44ec85070d24658a2e0901",
        count: 7,
        sort: "created: asc"
      },
      user: [],
      repos: []
    };
  },
  components: {
    Navbar,
    Profile,
    Repos
  },
  methods: {
    getUser(e) {
      const user = e.target.value;
      const { url, client_id, client_secret, count, sort } = this.github;
      axios
        .get(
          `${url}/${user}?client_id=${client_id}&client_secret=${client_secret}`
        )
        .then(({ data }) => (this.user = data));

      axios
        .get(
          `${url}/${user}/repos?per_page=${count}&sort=${sort}&client_id=${client_id}&client_secret=${client_secret}`
        )
        .then(({ data }) => (this.repos = data));
    }
  }
};
</script>