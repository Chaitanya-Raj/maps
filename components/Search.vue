<template>
  <div class="top-5 left-5 fixed">
    <ais-instant-search index-name="users" :search-client="searchClient">
      <ais-configure
        :attributesToSnippet="['fullName', 'location.city', 'location.country']"
        :hits-per-page.camel="20"
        snippetEllipsisText="…"
      >
        <ais-autocomplete>
          <template v-slot="{ currentRefinement, indices, refine }">
            <div class="rounded-xl flex p-4 bg-white border border-gray-400">
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
                class="ml-4 outline-none"
              />
            </div>
            <template v-if="currentRefinement">
              <ul
                v-for="index in indices"
                :key="index.indexId"
                class="max-h-96 p-2 my-2 overflow-y-scroll bg-white border border-gray-500 rounded-md"
              >
                <li
                  v-for="hit in index.hits"
                  :key="hit.objectID"
                  class="flex items-center gap-2 mb-2"
                >
                  <div class="flex items-center">
                    <img :src="hit.photo" alt="" class="rounded-full" />
                  </div>
                  <p class="flex flex-col">
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
      // return algoliaSearch(this.appID, this.searchKey);
    },
  },
  methods: {
    getSearchResults(items) {
      console.log(items);
    },
  },
};
</script>
