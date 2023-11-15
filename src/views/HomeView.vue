<template>
  <router-link to="/houseForm"><button>create</button></router-link>
  <input v-model="search" placeholder="search for a house" @input="filterHouses" />
  <button @click="sortByPrice">Price</button>
  <button @click="sortBySize">Size</button>
  <div class="home" v-for="house in houses">
    <router-link :to='{ name: "house-details", params: { id: house.id } }'>
      <img :src="house.image" class="house-image">
      <div class="house-info">
        <h1>city:{{ house.location.city }}</h1>
        <h1>street:{{ house.location.street }}</h1>
        <h1>number:{{ house.location.houseNumber }}</h1>
        <h1>Price: {{ house.price }}</h1>
        <h1>size:{{ house.size }}</h1>
        <h1>bed:{{ house.rooms.bedrooms }}</h1>
        <h1>bath:{{ house.rooms.bathrooms }}</h1>
      </div>
    </router-link>


  </div>
</template>

<script>
// @ is an alias to /src


export default {
  data() {
    return {
      houses: [],
      search: "",

    }
  },
  name: 'HomeView',
  components: {

  },
  methods: {
    getHouses() {
      this.houses = [];
      this.$store.dispatch("fetchApiData").then(() => {
        this.houses = this.$store.state.apiData;


      })
    },

    sortByPrice() {
      this.houses.sort((a, b) => a.price - b.price);
    },

    sortBySize() {
      this.houses.sort((a, b) => a.size - b.size);
    },



    filterHouses() {
      const searchTerm = this.search.toLowerCase();
      this.houses = this.$store.state.apiData.filter((house) => {
        return house.location.city.toLowerCase().includes(searchTerm)
          || house.location.zip.toLowerCase().includes(searchTerm)
          || house.size.toString().includes(searchTerm)
          || house.price.toString().includes(searchTerm);
      })

    }
  },

  computed: {
    filteredHouses() {
      const searchTerm = this.search.toLowerCase();
      return this.houses.filter((house) => {
        const cityMatch = house.location.city.toLowerCase().includes(searchTerm);
        const zipMatch = house.location.zip.toLowerCase().includes(searchTerm);


        const sizeMatch = house.size.toString().includes(searchTerm);
        const priceMatch = house.price.toString().includes(searchTerm);

        return cityMatch || zipMatch || sizeMatch || priceMatch;
      })
    }
  },



  mounted() {
    this.getHouses();
    console.log(this.houses);


  },

}
</script>
