<template>
  <div class="container">
    <h2>ALGOLIA + VUE2</h2>
    <ais-instant-search
      :search-client="searchClient"
      index-name="benchomatic-beta"
    >
      <div class="left-panel">
        <ais-clear-refinements />
        <h2>Title</h2>
        <ais-refinement-list
          attribute="title"
          searchable
          :attributesToSnippet="['title:10']"
        />
        <h2>Author</h2>
        <ais-refinement-list attribute="authors" searchable />
        <h2 class="mt-2">Publication year</h2>
        <ais-refinement-list attribute="publication_year" />
      </div>
      <ais-configure :hitsPerPage="8" :attributesToSnippet="['title']" />
      <div class="right-panel">
        <div class="searchbox">
          <ais-search-box />
        </div>
        <ais-hits>
          <template v-slot:item="{ item }">
            <div class="left-panel">
              <img :src="item.image_url" align="left" :alt="item.title" />
            </div>
            <div class="right-panel">
              <h2>
                <ais-highlight
                  attribute="title"
                  :hit="item"
                  highlightedTagName="mark"
                  :attributesToSnippet="['title:10']"
                />
              </h2>
              <div class="hit-authors">
                Authors:

                <span
                  v-for="(fieldValue, index) in item.authors"
                  :key="`highlight-${fieldValue}-${index}`"
                >
                  <ais-highlight :hit="item" :attribute="`authors.${index}`" />
                  <template v-if="index < item.authors.length - 1">,</template>
                </span>
              </div>

              <div class="hit-publication_year">
                Pub. year:
                <ais-highlight
                  attribute="publication_year"
                  :hit="item"
                ></ais-highlight>
              </div>
              <div class="hit-average_rating">
                Rating:
                {{ item.average_rating }}
              </div>
              <div class="hit-ratings_count">
                Total ratings:
                {{ item.ratings_count }}
              </div>
            </div>
          </template>
        </ais-hits>

        <ais-pagination />
        <!-- <ais-pagination>
        <template v-slot:first>first</template>
        <template v-slot:previous>prev</template>
        <template
          v-slot:item="{ value, active }"
          :style="{ color: active ? 'red' : 'green' }"
        >
          {{ value }}
        </template>
        <template v-slot:next>next</template>
        <template v-slot:last>last</template>
      </ais-pagination> -->
      </div>
    </ais-instant-search>
  </div>
</template>

<script>
import algoliasearch from "algoliasearch/lite";
import "instantsearch.css/themes/satellite.css";

export default {
  data() {
    return {
      searchClient: algoliasearch(
        "VMAF88UX7F",
        "fcbf79aa3cdc5840e379799095495acd"
      ),
    };
  },
};
</script>

<style>
body {
  font-family: sans-serif;
  padding: 1em;
}

.ais-Hits-list {
  margin-top: 0;
  margin-bottom: 1em;
}

.ais-InstantSearch {
  display: grid;
  grid-template-columns: 1fr 4fr;
  grid-gap: 1em;
}
.ais-Hits-item {
  display: grid;
  grid-template-columns: 1fr 5fr;
}
.ais-Hits-list {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
.ais-Hits-item img {
  margin-right: 1em;
}
.hit-name {
  margin-bottom: 0.5em;
}
.hit-description {
  color: #888;
  font-size: 0.8em;
  margin-bottom: 0.5em;
}
</style>
