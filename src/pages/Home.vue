<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <Search
          :value="search"
          placeholder="username"
          @search="search = $event"
        />
        <!-- принимаем emit -->
        <button @click="getGitRepos" class="btn btnPrimary">
          START SEARCH!
        </button>
        <!-- date -->
        <div class="repos-container">
          <div class="repos__wrapper" v-if="repos">
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
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
.repos-container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.btn {
  margin-top: 30px;
}
.repos__wrapper {
  min-width: 800px;
  margin: 30px 0;
  text-align: left;
}
.repos-info {
  display: flex;
  align-items: center;
  margin-bottom: 5px;
}
a.link {
  width: 40%;
  border-bottom: none;
  text-align: left;
}
</style>