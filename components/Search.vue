<template>
  <div class="fixed top-5 left-5">
    <ais-instant-search index-name="users" :search-client="searchClient">
      <ais-configure
        :attributesToSnippet="[
          'fullName',
          'designation',
          'location.city',
          'location.country',
        ]"
        :hits-per-page.camel="20"
        snippetEllipsisText="…"
      >
        <ais-autocomplete>
          <template v-slot="{ currentRefinement, indices, refine }">
            <div class="flex border border-gray-400 rounded-xl p-4 bg-white">
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
                class="bg-white max-h-96 rounded-md border-gray-500 border mt-2 p-4 overflow-y-scroll"
              >
                <li v-for="hit in index.hits" :key="hit.objectID" class="mb-2">
                  <!-- <h1>
                        <ais-highlight attribute="fullName" :hit="hit" />
                      </h1> -->
                  <!-- <h2>
                        <ais-highlight attribute="designation" :hit="hit" />
                      </h2> -->
                  <p class="flex flex-col">
                    <ais-snippet attribute="fullName" :hit="hit" />
                    <ais-snippet attribute="location.city" :hit="hit" />
                  </p>
                </li>
              </ul>
            </template>
          </template>
        </ais-autocomplete>
      </ais-configure>
    </ais-instant-search>
  </div>
</template>

<script>
import algoliaSearch from "algoliasearch/lite";
import "instantsearch.css/themes/algolia-min.css";

// configurations for Algolia search

export default {
  computed: {
    appID() {
      return this.$config.APP_ID;
    },
    searchKey() {
      return this.$config.SEARCH_API_KEY;
    },
    searchClient() {
      return algoliaSearch(
        // Application ID
        this.appID,

        // Search API key
        this.searchKey
      );
    },
  },
};
</script>
