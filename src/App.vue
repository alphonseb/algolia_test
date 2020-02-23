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
          <div
            class="search-panel__filters"
            :class="{ 'search-panel__filters--open': filtersOpen }"
          >
            <h2 class="search-panel__filters-title">Filter by</h2>
            <ais-clear-refinements
              :classNames="{
                'ais-ClearRefinements-button': 'search-panel__clear-button'
              }"
            >
              <span slot="resetLabel">Clear filters</span>
            </ais-clear-refinements>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Season
              </h3>
              <ais-refinement-list
                attribute="season"
                :classNames="{
                  'ais-RefinementList-count': 'search-panel__count'
                }"
                :sort-by="['name:desc']"
              />
            </div>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Winning Driver
              </h3>
              <ais-refinement-list
                attribute="Results.first.Driver.familyName"
                :classNames="{
                  'ais-RefinementList-count': 'search-panel__count'
                }"
              />
            </div>
            <div class="search-panel__single-filter">
              <h3 class="search-panel__subtitle">
                Winning Constructor
              </h3>
              <ais-refinement-list
                attribute="Results.first.Constructor.name"
                :classNames="{
                  'ais-RefinementList-count': 'search-panel__count'
                }"
              />
            </div>
          </div>

          <div class="search-panel__results">
            <button
              class="search-panel__show-filter"
              @click="filtersOpen = !filtersOpen"
            >
              <span v-if="!filtersOpen" class="search-panel__icon-container">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="#1f1f27"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  class="feather feather-filter"
                >
                  <polygon
                    points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"
                  ></polygon>
                </svg>
              </span>
              <span v-else class="search-panel__icon-container">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="#1f1f27"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  class="feather feather-x"
                >
                  <line x1="18" y1="6" x2="6" y2="18"></line>
                  <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
              </span>
            </button>
            <div class="search-panel__search-bar">
              <ais-search-box
                placeholder="German Grand Prix, Max Verstappen, Lewis, Circuit Paul Ricard..."
                class="searchbox"
                :classNames="{ 'ais-SearchBox-input': 'searchbox__input' }"
                show-loading-indicator
              />
              <ais-state-results>
                <p
                  class="search-panel__state-results"
                  slot-scope="{ nbHits, processingTimeMS }"
                >
                  {{ nbHits }} results found in {{ processingTimeMS }}ms
                </p>
              </ais-state-results>
            </div>
            <ais-hits :classNames="hitsClasses">
              <RaceItem :race="item" slot="item" slot-scope="{ item }" />
            </ais-hits>
            <div class="pagination">
              <ais-pagination
                :classNames="{
                  'ais-Pagination-item--selected': 'pagination__item--selected',
                  'ais-Pagination-link': 'pagination__link'
                }"
              />
            </div>
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
      ),

      filtersOpen: false
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
  --formula-red-light: hsl(2, 100%, 70%);
  --formula-black: #1f1f27;
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
p,
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
  align-items: flex-start;

  &__filters {
    flex: 1;
    margin-right: 1em;
    position: sticky;
    top: 32px;

    @media screen and (max-width: 600px) {
      background-color: #fff;
      padding: 1rem;
      position: fixed;
      z-index: 50;
      width: 50%;
      min-width: 270px;
      height: 100%;
      box-shadow: 0 10px 10px 0 #e3e5ec;
      border-top-right-radius: 8px;
      border-bottom-right-radius: 8px;
      top: 0;
      left: 0;
      transform: translateX(-100%);
      transition: transform 0.2s;

      &--open {
        transform: translateX(0);
      }
    }
  }

  &__show-filter {
    padding: 1rem;
    background-color: #fff;
    border: none;
    border-radius: 100%;
    box-shadow: 0 5px 5px 0 #e3e5ec;
    position: fixed;
    bottom: 32px;
    right: 32px;
    z-index: 100;
    display: none;
    text-align: center;
    cursor: pointer;

    @media screen and (max-width: 600px) {
      display: block;
    }
  }

  &__icon-container {
    display: block;
    width: 24px;
    height: 24px;
  }

  &__filters-title {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  &__clear-button {
    background-color: var(--formula-black);
    margin-bottom: 0.75rem;
    padding: 12px 16px;

    &:hover {
      opacity: 0.6;
      background-color: var(--formula-black);
    }
  }

  &__single-filter {
    margin-bottom: 1rem;
  }

  &__subtitle {
    font-family: "Formula1-Regular", sans-serif;
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
  }

  &__count {
    display: inline-block;
    text-align: center;
    background-color: var(--formula-red);
    opacity: 0.7;
    color: #fff;
    min-width: 27px;
    border-radius: 50px;
  }

  &__search-bar {
    margin-bottom: 1.75rem;
  }

  &__state-results {
    color: var(--formula-red);
    opacity: 0.7;
    font-size: 0.8rem;
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
    width: calc(100% / 2 - 1rem);
    min-width: 280px;

    @media screen and (max-width: 832px) {
      width: calc(100% - 1rem);
    }

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
      transition: transform 0.1s ease-in-out;
    }
  }
}

.searchbox {
  margin-bottom: 5px;

  &__input {
    padding-top: 12px;
    padding-bottom: 12px;
    padding-left: 32px;
    border: solid 2px var(--formula-black);
  }
}

.pagination {
  margin: 2rem auto;
  text-align: center;

  &__link {
    border-radius: 50px;
    color: var(--formula-red);
    padding: 0.1rem 0.4rem;

    &:hover {
      color: var(--formula-red);
    }
  }

  &__item--selected {
    .pagination__link {
      background-color: var(--formula-red);
      opacity: 0.7;
      border-color: var(--border-color);
    }
  }
}
</style>
