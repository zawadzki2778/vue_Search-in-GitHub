<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <div class="container-search">
          <Search
            :value="search"
            placeholder="username"
            @search="search = $event"
          />
          <!-- принимаем emit -->
          <button @click="getGitRepos" class="btn btnPrimary">SEARCH!</button>
        </div>

        <!-- date -->
        <div class="container-repos" v-if="repos">
          <div class="repos-item" v-for="repo in repos" :key="repo.id">
            <div class="repos-info">
              <a class="link" target="_blank" :href="repo.html_url">
                {{ repo.name }}
              </a>
              <div>🗓создан - &nbsp;</div>
              <p>{{ changDate(repo.created_at) }}</p>
            </div>
          </div>
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
    };
  },
  methods: {
    getGitRepos() {
      axios
        .get(`https://api.github.com/users/zawadzki2778/repos`)
        .then((res) => {
          this.repos = res.data;
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
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
  margin: 0 auto;
}
.container-repos {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  flex-wrap: wrap;
}
.btn {
  margin-top: 30px;
}
// .repos__wrapper {
//   min-width: 800px;
//   margin: 30px 0;
//   text-align: left;
// }
.repos-items {
  text-align: left;
  margin: 0 auto;
}
.repos-info {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 5px;
  width: 600px;
}
a.link {
  width: 40%;
  border-bottom: none;
  text-align: left;
}
</style>