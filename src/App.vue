<template>
  <div>
    <header class="header">
      <h1 class="header-title"><a href="/">Vue InstantSearch v2 starter</a></h1>
    </header>

    <div class="container">
      <ais-instant-search
        :search-client="searchClient"
        :stalled-search-delay="1000"
        :routing="routing"
        index-name="dev_fom_products"
      >
        <div class="search-panel">
          <div class="search-panel__filters">
            <h3>Marcas</h3>
            <ais-refinement-list attribute="Brand.title" />
            <h3>Categorías</h3>
            <ais-refinement-list attribute="division" />
            <h3>Tallas</h3>
            <ais-refinement-list attribute="sizes" :sort-by="['name:asc']" />
            <h3>Precio</h3>
            <ais-range-input attribute="precio_real" />
          </div>

          <div class="search-panel__results">
            <ais-search-box placeholder="Search here…" class="searchbox" />
            <ais-hits>
              <template slot="item" slot-scope="{ item }">
                <a :href="item.url">
                  <img v-if="item.images[0]" :src="item.images[0]" />
                  <h1><ais-highlight :hit="item" attribute="title" /></h1>
                  <p v-if="item.precio_real != item.precio" class="old">
                    {{ item.precio }}€
                  </p>
                  <p>{{ item.precio_real }}€</p>
                </a>
              </template>
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
import { history as historyRouter } from "instantsearch.js/es/lib/routers";
import { singleIndex as singleIndexMapping } from "instantsearch.js/es/lib/stateMappings";
import "instantsearch.css/themes/algolia-min.css";

export default {
  components: {},
  data() {
    return {
      searchClient: algoliasearch(
        "FHYQ0OII8R",
        "1f3ae756f8ee8e01f28fdc8b8775de84"
      ),
      routing: {
        router: historyRouter(),
        stateMapping: singleIndexMapping("dev_fom_products"),
      },
    };
  },
  methods: {},
};
</script>

<style>
body,
h1 {
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

.header {
  display: flex;
  align-items: center;
  min-height: 50px;
  padding: 0.5rem 1rem;
  background-image: linear-gradient(to right, #4dba87, #2f9088);
  color: #fff;
  margin-bottom: 1rem;
}

.header a {
  color: #fff;
  text-decoration: none;
}

.header-title {
  font-size: 1.2rem;
  font-weight: normal;
}

.header-title::after {
  content: " ▸ ";
  padding: 0 0.5rem;
}

.header-subtitle {
  font-size: 1.2rem;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem;
}

.search-panel {
  display: flex;
}

.search-panel__filters {
  flex: 1;
  margin-right: 1em;
}

.search-panel__results {
  flex: 3;
}

.searchbox {
  margin-bottom: 2rem;
}

.pagination {
  margin: 2rem auto;
  text-align: center;
}

img {
  width: 100%;
}

.old {
  text-decoration: line-through;
}
</style>
