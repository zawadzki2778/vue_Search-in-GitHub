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

        <div class="repos__wrapper" v-if="repos">
          <div class="repos-item" v-for="repo in repos" :key="repo.id">
            <div class="repos-info">
              <a class="link" target="_blank" :href="repo.html_url">
                {{ repo.name }}
              </a>
              <div class="repos-date">
                <span> создан - {{ changDate(repo.created_at) }} 🗓 </span>
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
      const normalDate = dateObj.toLocaleString();
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
.btn {
  margin-top: 30px;
}
.repos__wrapper {
  width: 800px;
  margin: 30px 0;
}
.repos-info {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  margin-bottom: 5px;
}
.repos-date {
  display: flex;
  text-align: left;
}
</style>