<template>
  <div>
    <div class="pt-2 relative text-gray-600 w-full">
      <input
        class="border-2 border-gray-300 bg-white h-10 px-5 w-full rounded-lg text-sm focus:outline-none"
        type="search"
        name="search"
        v-model="search"
        placeholder="Search"
      />
    </div>

    <div
      class="fixed z-10 inset-0 overflow-y-auto "
      aria-labelledby="modal-title"
      role="dialog"
      aria-modal="true"
      v-if="showModal"
    >
      <div
        class="flex items-end justify-center min-h-screen px-4 pb-4 text-center sm:block sm:p-0"
      >
        <div
          class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
          aria-hidden="true"
        ></div>

        <span
          class="hidden sm:inline-block sm:align-middle sm:h-screen"
          aria-hidden="true"
          >&#8203;</span
        >

        <div
          class="inline-block bg-white rounded-lg overflow-hidden shadow-xl transform transition-all my-8 align-middle max-w-screen-md w-full"
        >
          <div class="bg-white px-16 pt-8 pb-8 ">
            <div class="text-center ">
              <div class="mt-3 text-center ">
                <h3
                  class="text-3xl leading-6 font-medium  text-center text-gray-900"
                  id="modal-title"
                >
                  {{ nameRocket }}
                </h3>
                <div class="mt-4">
                  <p class="text-sm text-gray-500  text-center">
                    {{ dateTime }}
                  </p>
                </div>
              </div>
            </div>
            <div class=" border-b-2 mt-4 mb-4"></div>
            <div v-if="crews.length !== 0">
              <div class="items-center text-center flex ">
                <div
                  class="w-24 mx-auto mb-3 rounded-full mt-3 bg-indigo-600 text-white"
                >
                  Crews
                </div>
              </div>
              <div>
                <div class="flex items-center ">
                  <div
                    v-for="(item, index) in personsImage"
                    :key="index"
                    class="mx-auto"
                  >
                    <img
                      class=" inline-block h-40 w-40 rounded-full ring-2 ring-white  "
                      :src="item"
                    />
                  </div>
                </div>
                <div>
                  <div class="items-center text-center flex mt-4">
                    <div
                      v-for="item in personsname"
                      :key="item.index"
                      class="w-full mx-auto mb-3"
                    >
                      {{ item }}
                    </div>
                  </div>
                </div>
              </div>
              <div class=" border-b-2 mt-4 mb-4"></div>
            </div>
            <div class="items-center text-center ">
              <div
                class="w-24 mx-auto  rounded-full mt-3 bg-indigo-600 text-white"
              >
                Rocket
              </div>
              <div class="mt-3">
                {{ nameRockets }}
              </div>
            </div>
            <div
              v-for="item in imageRocket"
              :key="item.index"
              class="mb-3 my-3"
            >
              <img :src="item" class="rounded-md" alt="" />
            </div>
            <div class=" border-b-2 mt-4 mb-4"></div>

            <div>
              <div class="text-center ">
                <div class="mt-3 text-center ">
                  <h3
                    class="text-3xl leading-6 font-medium  text-center text-gray-900"
                    id="modal-title"
                  >
                    Launched
                  </h3>
                  <div class="mt-4">
                    <div class="text-md   text-center">
                      <div>
                        Name :
                        <span class="text-sm text-gray-500">
                          {{ launchpads.name }}
                        </span>
                      </div>
                      <div>
                        Full name :
                        <span class="text-sm text-gray-500">
                          {{ launchpads.full_name }}
                        </span>
                      </div>
                      <div>
                        Region :
                        <span class="text-sm text-gray-500">
                          {{ launchpads.region }}
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class=" border-b-2 mt-4"></div>
          </div>

          <div class=" px-4 py-6 sm:px-6 sm:flex sm:flex-row-reverse">
            <button
              type="button"
              @click="closeModal()"
              class=" w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
    <div
      v-for="(item, index) in filteredItems"
      :key="index"
      :index="index"
      class="flex border shadow dark:bg-gray-800 hover:shadow-md bg-white p-6 mt-4"
      @click="toggleModal(index)"
    >
      <div class="w-1/2 ">
        <div>
          <span class="flex" v-if="item.links.patch.small !== null">
            <img
              class="w-12"
              :src="item.links.patch.small"
              alt="Italian Trulli"
            />
            <span class="pt-3 pl-4">{{ item.name }}</span>
          </span>
          <span class="flex" v-else>
            <img
              class="w-12"
              src="data:image/png;base64,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"
              alt="Italian Trulli"
            />
            <span class="pt-4 pl-4 mr-4">{{ item.name }}</span>
          </span>
        </div>
      </div>
      <div class="w-1/2  relative text-center item-right  ">
        <div class="absolute inset-y-0 right-0 flex">
          <div v-if="item.crew.length !== 0" class="">
            <div
              class="item-right w-24 mr-4 rounded-full mt-3 bg-indigo-600 text-white"
            >
              {{ item.crew.length }} crews
            </div>
          </div>
          <div>
            <div class="mt-3 mr-4">{{ item.date_local | moment }}</div>
          </div>
          <div v-if="item.upcoming === true" class="">
            <div class="item-right w-24 rounded-full text-indigo-600 mt-3 mr-4">
              Upcoming
            </div>
          </div>
          <div v-else>
            <div class="item-right w-24 rounded-full text-indigo-600 mt-3 mr-4">
              Launched
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "regular-modal",
  props: { nametabs: String, posts: Array },
  data() {
    return {
      showModal: false,
      date: "",
      nameRocket: "",
      dateTime: "",
      idRocket: "",
      nameRockets: "",
      launchpadsId: "",
      imageRocket: [],
      personRocket: [],
      personsname: [],
      personsImage: [],
      postq: [],
      launchpads: [],
      crews: [],
      rocket: [],
      selected: "",
      fillterName: [{ name: "Name" }, { name: "Date" }],
      search: "",
      items: [
        { name: "Stackoverflow", type: "development" },
        { name: "Game of Thrones", type: "serie" },
        { name: "Jon Snow", type: "actor" },
      ],
    };
  },
  computed: {
    filteredItems() {
      return this.posts.filter((item) => {
        return item.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
      });
    },
  },
  methods: {
    fillterBy(item) {
      this.search = item;
    },
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      (this.personsname = []), (this.personsImage = []);
      this.showModal = false;
    },
    async toggleModal(item) {
      this.idRocket = this.posts[item].rocket;
      this.crews = this.posts[item].crew;
      this.postq.push(this.posts[item].name);
      this.launchpadsId = this.posts[item].launchpad;
      await axios
        .get(`https://api.spacexdata.com/v4/launchpads/${this.launchpadsId}`)
        .then((response) => {
          this.launchpads = response.data;
        });
      await axios
        .get(`https://api.spacexdata.com/v4/rockets/${this.idRocket}`)
        .then((response) => {
          this.rocket = response.data;
        });

      for (const dataCrew in this.crews) {
        await axios
          .get(`https://api.spacexdata.com/v4/crew/${this.crews[dataCrew]}`)
          .then((response) => {
            this.personRocket = response.data;
            this.personsname.push(this.personRocket.name);
            this.personsImage.push(this.personRocket.image);
          });
      }
      this.showModal = true;
      this.nameRocket = this.posts[item].name;
      this.dateTime = moment(this.posts[item].date_local).format("ddd lll");
      this.nameRockets = this.rocket.name;
      this.imageRocket = this.rocket.flickr_images;
    },
    moment() {
      return moment();
    },
  },
  filters: {
    moment(date) {
      return moment(date)
        .format("ddd ll")
        .replace(",", "");
    },
  },
};
</script>

<style></style>
