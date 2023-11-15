<template>
  <div>
    <button @click="goToEditPage">edit</button>
    <button>delete</button>
    <h1>{{ house.street }}</h1>
    <h1>{{ house.number }}</h1>
    <h1>{{ house.addition }}</h1>
    <h1>{{ house.postalCode }}</h1>
    <h1>{{ house.city }}</h1>
    <img :src="imageUrl">
    <h1>{{ house.price }}</h1>
    <h1>{{ house.size }}</h1>
    <h1>{{ house.garage }}</h1>
    <h1>{{ house.bedrooms }}</h1>
    <h1>{{ house.bathrooms }}</h1>
    <h1>{{ house.constructionDate }}</h1>
    <h1>{{ house.details }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      house: {},
      imageUrl: null
    }
  },



  mounted() {
    const houseIndex = this.$route.params.index;
    const selectedHouse = this.$store.state.myListings[houseIndex];

    if (selectedHouse) {
      this.house = selectedHouse;
      this.loadImage();
    } else {
      // Handle the case where the house is not found, e.g., redirect to an error page
      console.error(`House at index ${houseIndex} not found.`);
    }
  },

  methods: {
    loadImage() {
      const reader = new FileReader();
      reader.onload = () => {
        this.imageUrl = reader.result;
      };

      // Assuming that house.image is the File object
      reader.readAsDataURL(this.house.image);
    },

    goToEditPage() {
      const index = this.$store.state.myListings.length - 1;

            // Redirect to the details page with the index as a parameter
            this.$router.push({ name: 'edit-house', params: { index } });

    }
  },
}

</script>