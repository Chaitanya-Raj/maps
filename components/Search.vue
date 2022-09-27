<template>
  <div class="search-cont inline-flex gap-2 bg-white p-2 rounded-lg shadow-lg">
    <ais-instant-search index-name="users" :search-client="searchClient">
      <ais-configure
        :attributesToSnippet="['fullName', 'designation']"
        :hits-per-page.camel="5"
        snippetEllipsisText="…"
      >
        <ais-autocomplete>
          <template v-slot="{ currentRefinement, indices, refine }">
            <input
              type="search"
              :value="currentRefinement"
              placeholder="Search Members"
              @input="refine($event.currentTarget.value)"
            />
            <ais-stats />
            <template v-if="currentRefinement">
              <ul v-for="index in indices" :key="index.indexId">
                <li>
                  <h3>{{ index.indexName }}</h3>
                  <ul>
                    <li v-for="hit in index.hits" :key="hit.objectID">
                      <!-- <h1>
                        <ais-highlight attribute="fullName" :hit="hit" />
                      </h1> -->
                      <!-- <h2>
                        <ais-highlight attribute="designation" :hit="hit" />
                      </h2> -->
                      <p class="flex flex-col p-2 border-black border">
                        <ais-snippet attribute="fullName" :hit="hit" />
                        <ais-snippet attribute="designation" :hit="hit" />
                      </p>
                    </li>
                  </ul>
                </li>
              </ul>
            </template>
            <ais-pagination />
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
