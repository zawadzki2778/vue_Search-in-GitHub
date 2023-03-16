<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <Search :value="search" placeholder="username" @search="search = $event" /> <!-- принимаем emit -->
        <button class="btn btnPrimary">START SEARCH!</button>
      </div>
    </section>
  </div>
</template>

<script>
import Search from "@/components/Search.vue";
export default {
  name: "Home",
  components: { Search },
  data() {
    return {
      search: "",
    };
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
</style>