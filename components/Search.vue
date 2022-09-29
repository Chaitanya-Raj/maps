<template>
  <div class="top-5 left-5 w-96 fixed">
    <ais-instant-search index-name="users" :search-client="searchClient">
      <ais-configure
        :attributesToSnippet="['fullName', 'location.city', 'location.country']"
        :hits-per-page.camel="500"
        snippetEllipsisText="…"
      >
        <ais-autocomplete>
          <template v-slot="{ currentRefinement, indices, refine }">
            <div
              class="rounded-xl opacity-90 flex p-4 transition-all bg-white border border-gray-400 shadow-lg"
            >
              <!-- <a
              href="https://www.flaticon.com/free-icons/search"
              title="search icons"
              >Search icons created by Catalin Fertu - Flaticon</a
            > -->
              <div class="flex items-center">
                <img src="search-interface-symbol.png" alt="" class="h-5" />
              </div>
              <input
                type="search"
                :value="currentRefinement"
                placeholder="Search Members"
                @input="refine($event.currentTarget.value)"
                class="font-poppins w-full ml-4 outline-none opacity-100"
              />
            </div>
            <template v-if="currentRefinement">
              <ul
                v-for="index in indices"
                :key="index.indexId"
                class="max-h-96 opacity-90 p-2 my-2 overflow-y-scroll bg-white border border-gray-500 rounded-lg shadow-lg"
              >
                <li
                  v-for="hit in index.hits"
                  :key="hit.objectID"
                  class="flex items-center gap-2 mb-2 cursor-pointer"
                  @click="
                    $emit('fly', [hit['location.lng'], hit['location.lat']])
                  "
                >
                  <div class="flex items-center">
                    <img
                      :src="hit.photo"
                      alt=""
                      class="rounded-full opacity-100"
                    />
                  </div>
                  <p class="font-poppins flex flex-col opacity-100">
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
  },
};
</script>

<style>
*::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  background-color: #f5f5f5;
}

*::-webkit-scrollbar {
  width: 6px;
  background-color: #f5f5f5;
}

*::-webkit-scrollbar-thumb {
  background-color: #000000;
}
</style>
