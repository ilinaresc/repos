<template>
  <q-page class="q-pa-md">
    <div class="q-pa-md bg-white">
      <q-input
        filled
        v-model="searchQuery"
        placeholder="Buscar programas..."
        class="q-mb-md"
        debounce="300"
        @input="filterProducts"
        prepend-inner-icon="search"
      />

      <div class="row q-col-gutter-md q-mb-md">
        <div class="col-xs-12 col-sm-6 col-md-4">
          <q-select
            filled
            v-model="selectedPriceRange"
            :options="priceRanges"
            label="Filtrar por promedio"
            @update:model-value="filterProducts"
            class="q-mb-md"
          />
        </div>
      </div>
    </div>

    <div class="row q-col-gutter-md q-mt-md">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="col-xs-12 col-sm-6 col-md-4 col-lg-3"
      >
        <q-card class="bg-purple-6 text-white">
          <q-img :src="product.image" alt="product.name" class="q-img-cover" />
          <q-card-section>
            <div class="text-h6">{{ product.name }}</div>
            <div class="text-subtitle1"> {{ product.price }}</div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "ProductsPage",
      params: {
      api_key: "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjODRmMmUwODYxM2JmN2FlYTM1OGI0OTgzNDNkMWUwNiIsInN1YiI6IjVmZTUxM2M3ZTE4Yjk3MDAzYzg5NzE3MCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.StJwmra-PsBwZTOXWg3Y06VEu8CGfAo8dXV7ZQ3RnIs",
    },
  data() {
    return {
      searchQuery: "",
      selectedPriceRange: null,
      priceRanges: [
        { label: "Todos", value: null },
        { label: "0 - 2.5", value: [0, 2.5] },
        { label: "2.5 - 5", value: [2.5, 5] },
        { label: "5 - 7.5", value: [5, 7.5] },
        { label: "7.5 - 10", value: [7.5, 10] },
      ],
      products: [
                {
            "original_name": "The Boys",
            "poster_path": "/2zmTngn1tYC1AvfnrFLhxeD82hz.jpg",
            "vote_average": 8.475,
            "vote_count": 9899
        },
        {
            "original_name": "Law & Order: Special Victims Unit",
            "poster_path": "/onmSVwYsPMYtO8OjLdjS8FfRNKb.jpg",
            "vote_average": 7.9,
            "vote_count": 3727
        },
        {
            "original_name": "House of the Dragon",
            "poster_path": "/7QMsOTMUswlwxJP0rTTZfmz2tX2.jpg",
            "vote_average": 8.419,
            "vote_count": 4359
        },
        {
            "original_name": "Diners, Drive-Ins and Dives",
            "poster_path": "/1ulyar2gfSnPaEDIyaT4f9zrhHr.jpg",
            "vote_average": 6.598,
            "vote_count": 56
        },
        {
            "original_name": "Grey's Anatomy",
            "poster_path": "/jcEl8SISNfGdlQFwLzeEtsjDvpw.jpg",
            "vote_average": 8.231,
            "vote_count": 10067
        },
        {
            "original_name": "CSI: Crime Scene Investigation",
            "poster_path": "/i5hmoRjHNWady4AtAGICTUXknKH.jpg",
            "vote_average": 7.608,
            "vote_count": 1196
        },
        {
            "original_name": "WWE Raw",
            "poster_path": "/qvQ2Fu75bNUiZ1yzuHI3wkTGy7C.jpg",
            "vote_average": 6.631,
            "vote_count": 308
        },
        {
            "original_name": "Hell's Kitchen",
            "poster_path": "/shOVFku8daIGRpgjTdMZBxUAvsV.jpg",
            "vote_average": 6.874,
            "vote_count": 265
        },
        {
            "original_name": "Top Chef VIP",
            "poster_path": "/6Gy1ReRZ9sK9g8TPXZGz7CcQvrV.jpg",
            "vote_average": 5.625,
            "vote_count": 8
        },
        {
            "original_name": "SOKO Köln",
            "poster_path": "/uCerPXtj8Oz9kvks9KuUE16O0I4.jpg",
            "vote_average": 6.2,
            "vote_count": 4
        },
        {
            "original_name": "No Rancho Fundo",
            "poster_path": "/eONkvEahSQJan1HTzWJKjvaMe29.jpg",
            "vote_average": 5.179,
            "vote_count": 14
        },
        {
            "original_name": "Suidooster",
            "poster_path": "/naCgSiacvV685kait6fBvhVhdce.jpg",
            "vote_average": 7.833,
            "vote_count": 12
        },
        {
            "original_name": "Game of Thrones",
            "poster_path": "/1XS1oqL89opfnbLl8WnZY1O1uJx.jpg",
            "vote_average": 8.453,
            "vote_count": 23432
        },
        {
            "original_name": "The Real World",
            "poster_path": "/pqeqlmK1KEBfEfABnPjEr7oXjWL.jpg",
            "vote_average": 5.625,
            "vote_count": 36
        },
        {
            "original_name": "The Good Doctor",
            "poster_path": "/53P8oHo9cfOsgb1cLxBi4pFY0ja.jpg",
            "vote_average": 8.491,
            "vote_count": 12201
        },
        {
            "original_name": "Murdoch Mysteries",
            "poster_path": "/bGh2JSvl2Yb7wcK8UVVjW8Tjb5U.jpg",
            "vote_average": 7.7,
            "vote_count": 237
        },
        {
            "original_name": "It's Always Sunny in Philadelphia",
            "poster_path": "/pRWO6ufqSNkWvPXDDQhBwPNSv4K.jpg",
            "vote_average": 8.312,
            "vote_count": 1024
        },
        {
            "original_name": "The Flying Doctors",
            "poster_path": "/Aj4xa7lo9RwTV6DpZ3Pcjgp26Ht.jpg",
            "vote_average": 6.5,
            "vote_count": 13
        },
        {
            "original_name": "The Real Housewives of Orange County",
            "poster_path": "/mEg3ohdBkLOs0L1GmYGtiwyJXlE.jpg",
            "vote_average": 5.5,
            "vote_count": 60
        },
        {
            "original_name": "The Tonight Show Starring Jimmy Fallon",
            "poster_path": "/d735cGL2G07Tb0ysLHStu8c0uo8.jpg",
            "vote_average": 5.83,
            "vote_count": 309
        }
      ],
      filteredProducts: [],
    };
  },
  mounted() {
    this.filterProducts();
  },
  methods: {
    filterProducts() {
      let filtered = this.products;

      if (this.searchQuery) {
        filtered = filtered.filter((product) =>
          product.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }

      if (this.selectedPriceRange && this.selectedPriceRange.value) {
        const [min, max] = this.selectedPriceRange.value;
        filtered = filtered.filter(
          (product) => product.vote_average >= min && product.vote_average <= max
        );
      }

      this.filteredProducts = filtered;
    },
  },
  watch: {
    searchQuery() {
      this.filterProducts();
    },
    selectedPriceRange() {
      this.filterProducts();
    },
  },
};
</script>

<style scoped></style>