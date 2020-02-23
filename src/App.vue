<template>
  <div>
    <Header />

    <div class="container">
      <ais-instant-search
        :search-client="searchClient"
        index-name="dev_FORMULA1"
      >
        <ais-configure :hits-per-page.camel="18" />
        <div class="search-panel">
          <div class="search-panel__filters">
            <h2>Filter by</h2>
            <ais-clear-refinements>
              <span slot="resetLabel">Clear filters</span>
            </ais-clear-refinements>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Season
              </h3>
              <ais-refinement-list attribute="season" />
            </div>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Winning Driver
              </h3>
              <ais-refinement-list
                attribute="Results.first.Driver.familyName"
              />
            </div>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Winning Constructor
              </h3>
              <ais-refinement-list attribute="Results.first.Constructor.name" />
            </div>
          </div>

          <div class="search-panel__results">
            <ais-search-box
              placeholder="German Grand Prix, Max Verstappen, Lewis..."
              class="searchbox"
              :classNames="{ 'ais-SearchBox-input': 'searchbox__input' }"
            />
            <ais-hits :classNames="hitsClasses">
              <RaceItem :race="item" slot="item" slot-scope="{ item }" />
            </ais-hits>
            <div class="pagination"><ais-pagination /></div>
          </div>
        </div>
      </ais-instant-search>
    </div>
  </div>
</template>

<script>
import algoliasearch from "algoliasearch/lite";
import "instantsearch.css/themes/algolia-min.css";

import RaceItem from "@/components/RaceItem.vue";
import Header from "@/components/Header.vue";

export default {
  data() {
    return {
      searchClient: algoliasearch(
        "HD5GJZDE5L",
        "23a954bbceb8011b7c0cd562c5c63f09"
      )
    };
  },

  computed: {
    hitsClasses() {
      return {
        "ais-Hits-item": "search-panel__hit"
      };
    }
  },

  components: {
    RaceItem,
    Header
  }
};
</script>

<style lang="scss">
:root {
  --formula-red: #e10600;
}

@font-face {
  font-family: "Formula1-Bold";
  src: url("~@/assets/fonts/Formula1-Bold.otf") format("opentype");
  font-weight: bold;
  font-style: normal;
}

@font-face {
  font-family: "Formula1-Regular";
  src: url("~@/assets/fonts/Formula1-Regular.otf") format("opentype");
  font-weight: normal;
  font-style: normal;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body,
h1,
h2,
h3,
h4,
h5 {
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem;
}

.search-panel {
  display: flex;

  &__filters {
    flex: 1;
    margin-right: 1em;
  }

  &__subtitle {
    font-family: "Formula1-Regular", sans-serif;
  }

  &__results {
    flex: 3;
  }

  &__hit {
    padding: 0;
    border-radius: 8px;
    border: none;
    transition: transform 0.2s ease-in-out;
    position: relative;
    width: calc(100% / 3 - 1rem);
    min-width: 250px;

    &:hover {
      transform: translateY(-5px);

      &::before {
        transform: scale(1);
      }
    }

    &:active {
      transform: translateY(0);
      transition-duration: 0.1s;

      &::before {
        transform: scale(0.65);
      }
    }

    &::before {
      content: "";
      background: none;
      width: 100%;
      height: 100%;
      position: absolute;
      z-index: 5;
      left: 0;
      top: 0;
      box-shadow: 0 10px 10px 0 #e3e5ec;
      transform: scale(0.65);
      transform-origin: 50% 50%;
      transition: transform 0.2s ease-in-out;
    }
  }
}

.searchbox {
  margin-bottom: 2rem;

  &__input {
    padding-top: 12px;
    padding-bottom: 12px;
    padding-left: 32px;
  }
}

.pagination {
  margin: 2rem auto;
  text-align: center;
}
</style>
