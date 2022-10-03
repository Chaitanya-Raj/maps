<template>
  <div class="top-5 left-5 w-96 fixed">
    <ais-instant-search index-name="users" :search-client="searchClient">
      <ais-configure
        :attributesToSnippet="['fullName', 'location.city', 'location.country']"
        :hits-per-page.camel="1000"
        snippetEllipsisText="…"
      >
        <ais-autocomplete>
          <template v-slot="{ currentRefinement, indices, refine }">
            <div
              class="opacity-90 bg-offWhite flex p-4 transition-all rounded-lg shadow-lg"
            >
              <div class="flex items-center">
                <img src="search-interface-symbol.png" alt="" class="h-5" />
              </div>
              <!-- TODO: Make more responsive -->
              <input
                type="search"
                :value="currentRefinement"
                placeholder="Search Members"
                @input="refine($event.currentTarget.value)"
                class="font-mulish bg-offWhite w-full ml-4 transition-all outline-none opacity-100"
              />
            </div>
            <template v-if="currentRefinement">
              <ul
                v-for="index in indices"
                :key="index.indexId"
                class="max-h-96 opacity-90 bg-offWhite scroll-smooth p-4 my-2 overflow-y-scroll transition-all rounded-lg shadow-lg"
              >
                <li
                  v-for="hit in index.hits"
                  :key="hit.objectID"
                  class="flex items-center gap-2 mb-4 cursor-pointer"
                  @click="() => showUser(hit)"
                >
                  <div class="flex items-center">
                    <!-- :src="`https://ui-avatars.com/api/?background=${'000000'.replace(
                        /0/g,
                        function () {
                          return (~~(Math.random() * 16)).toString(16);
                        }
                      )}&color=fff&name=${hit.fullName.split(' ')[0]}+${
                        hit.fullName.split(' ')[1]
                      }`" -->
                    <img
                      :src="hit.photo"
                      alt=""
                      class="w-12 rounded-full opacity-100"
                    />
                  </div>
                  <p class="font-mulish flex flex-col opacity-100">
                    <span class="text-lg">{{ hit.fullName }}</span>
                    <span class="text-gray-600"
                      >{{ hit["location.city"] }},
                      {{ hit["location.country"] }}</span
                    >
                  </p>
                </li>
              </ul>
            </template>
          </template>
        </ais-autocomplete>
        <!-- <ais-stats class="opacity-80 mt-1 ml-1" /> -->
        <!-- <ais-pagination>
          <template
            v-slot="{
              currentRefinement,
              nbPages,
              pages,
              isFirstPage,
              isLastPage,
              refine,
              createURL,
            }"
          >
            <ul class="opacity-90 flex p-2 bg-white rounded-full">
              <li v-if="!isFirstPage">
                <a :href="createURL(0)" @click.prevent="refine(0)"> ‹‹ </a>
              </li>
              <li v-if="!isFirstPage">
                <a
                  :href="createURL(currentRefinement - 1)"
                  @click.prevent="refine(currentRefinement - 1)"
                >
                  ‹
                </a>
              </li>
              <li v-for="page in pages" :key="page">
                <a
                  :href="createURL(page)"
                  :style="{
                    fontWeight: page === currentRefinement ? 'bold' : '',
                  }"
                  @click.prevent="refine(page)"
                >
                  {{ page + 1 }}
                </a>
              </li>
              <li v-if="!isLastPage">
                <a
                  :href="createURL(currentRefinement + 1)"
                  @click.prevent="refine(currentRefinement + 1)"
                >
                  ›
                </a>
              </li>
              <li v-if="!isLastPage">
                <a :href="createURL(nbPages)" @click.prevent="refine(nbPages)">
                  ››
                </a>
              </li>
            </ul>
          </template>
        </ais-pagination> -->

        <ais-hits class="hidden" :transform-items="getSearchResults" />
      </ais-configure>
    </ais-instant-search>
  </div>
</template>

<script>
import algoliaSearch from "algoliasearch/lite";

// configurations for Algolia search

export default {
  data() {
    return {
      appID: this.$config.APP_ID,
      searchKey: this.$config.SEARCH_API_KEY,
    };
  },
  computed: {
    searchClient() {
      return algoliaSearch(this.appID, this.searchKey);
    },
  },
  methods: {
    getSearchResults(items) {
      this.$emit("search", items);
    },
    showUser(hit) {
      this.$emit("fly", [hit["location.lng"], hit["location.lat"]]);
      this.$emit("show-modal", hit);
    },
  },
};
</script>

<style>
*::-webkit-scrollbar-track {
  /* -webkit-box-shadow: inset 0 0 6px #9dcdee;
  box-shadow: inset 0 0 6px #9dcdee; */
  background-color: #f5f5f5;
}

*::-webkit-scrollbar {
  width: 6px;
  background-color: #f5f5f5;
}

*::-webkit-scrollbar-thumb {
  background-color: #9dcdee;
}
</style>
