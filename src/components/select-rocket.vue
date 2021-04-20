<template>
  <div>
    <div class="flex mt-10">
      <div
        class="w-full"
        v-for="item in items"
        :key="item.message"
        @click="tabCollect(item.message)"
      >
        <a
          class=" text-center w-full flex  items-center px-5 py-3 border border-transparentrounded-md bg-gray-400 hover:border-2  hover:border-gray-500 hover:bg-indigo-700 "
          :class="{
            'bg-indigo-600 shadow-lg hover:border-gray-300 hover:shadow-lg hover:border-transparent  ':
              item.message === nametabs,
          }"
        >
          <p class=" text-base font-medium text-white text-center">
            <span class="text-center"> {{ item.message }} </span>
          </p>
        </a>
      </div>
    </div>
    <table-data :nametabs="nametabs" :posts="posts" />
  </div>
</template>

<script>
import tableData from "./table-data";
import axios from "axios";

export default {
  data() {
    return {
      items: [
        { message: "All" },
        { message: "Launched" },
        { message: "Upcoming" },
      ],
      nametabs: "All",
      posts: [],
    };
  },
  components: { tableData },
  async mounted() {
    await axios
      .get(`https://api.spacexdata.com/v4/launches`)
      .then((response) => {
        this.posts = response.data;
      });

    console.log(this.sortItem);
  },
  methods: {
    async tabCollect(message) {
      this.nametabs = message;
      if (this.nametabs === "All") {
        await axios
          .get(`https://api.spacexdata.com/v4/launches`)
          .then((response) => {
            this.posts = response.data;
          });
      } else if (this.nametabs === "Launched") {
        await axios
          .get(`https://api.spacexdata.com/v4/launches/past`)
          .then((response) => {
            this.posts = response.data;
          });
      } else {
        await axios
          .get(`https://api.spacexdata.com/v4/launches/upcoming`)
          .then((response) => {
            this.posts = response.data;
          });
      }
    },
  },
};
</script>

<style></style>
