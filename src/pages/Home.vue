<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- error -->
        <div class="error" style="color: hsl(0, 100%, 50%)" v-if="error">
          <p>{{ error }}</p>
        </div>

        <div class="container-search">
          <Search
            :value="search"
            placeholder="username"
            @search="search = $event"
          />

          <!-- принимаем emit -->
          <button @click="getGitRepos" class="btn btnPrimary">SEARCH!</button>
        </div>
        <div class="container-table">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th style="font-size: 20px" class="text-left">
                    Name of repositories
                  </th>
                  <th style="font-size: 20px" class="text-left">
                    Date of creation
                  </th>
                </tr>
              </thead>
              <tbody v-if="repos">
                <tr v-for="repo in repos" :key="repo.id">
                  <td style="font-size: 16px">
                    <a class="link" target="_blank" :href="repo.html_url">
                      {{ repo.name }}
                    </a>
                  </td>
                  <td style="font-size: 16px">
                    {{ changDate(repo.created_at) }}
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Search from "@/components/Search.vue";
import axios from "axios";
export default {
  name: "Home",
  components: { Search },
  data() {
    return {
      search: "",
      repos: null,
      error: null,
    };
  },
  methods: {
    getGitRepos() {
      axios
        .get(`https://api.github.com/users/${this.search}/repos`)
        .then((res) => {
          (this.err = null), (this.repos = res.data);
          // console.log(res);
        })
        .catch((err) => {
          this.repos = null;
          this.error = "Can`t found this user";
          // console.log(err);
        });
    },
    changDate(isoDate) {
      const dateObj = new Date(isoDate);
      const normalDate = dateObj.toLocaleString().slice(0, -3);
      return normalDate;
    },
  },
};
</script>

<style lang="scss" scope>
.container-search {
  display: flex;
  align-items: baseline;
  justify-content: center;
  margin-right: 220px;
}
.container-table {
  margin: 40px auto;
  max-width: 70%;
}

.btn {
  margin-left: 100px;
}

a.link {
  width: 40%;
  border-bottom: none;
  text-align: left;
  font-size: 18px;
}
.error {
  text-align: center;
  margin: 0 80px 20px 0;
}
</style>