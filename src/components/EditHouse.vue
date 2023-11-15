<template>
    <div>
        <h1>Edit page</h1>

        <form @submit.prevent="saveChanges">
            <label for="streetName">House Name:</label>
            <input v-model="house.street" type="text" id="streetName">

            <label for="houseNumber">House Number:</label>
            <input v-model="house.number" type="number" id="houseNumber">

            <label for="houseAddition">House Addition:</label>
            <input v-model="house.addition" type="text" id="houseAddition">

            <label for="housePostalCode">House Postal Code:</label>
            <input v-model="house.postalCode" type="number" id="housePostalCode">

            <label for="houseCity">House City:</label>
            <input v-model="house.city" type="text" id="houseCity">

            <label for="houseImage">House Image:</label>
            <img :src="imageUrl">
            <input type="file" id="houseImage" @change="handleImageUpload">

            <label for="housePrice">House Price:</label>
            <input v-model="house.price" type="number" id="housePrice">

            <label for="houseSize">House Size:</label>
            <input v-model="house.size" type="number" id="houseSize">

            <label for="houseGarage">House Garage:</label>
            <input v-model="house.garage" type="text" id="houseGarage">

            <label for="houseBedrooms">House Bedrooms:</label>
            <input v-model="house.bedrooms" type="number" id="houseBedrooms">

            <label for="houseBathrooms">House Bathrooms:</label>
            <input v-model="house.bathrooms" type="number" id="houseBathrooms">

            <label for="houseConstructionDate">House Construction Date:</label>
            <input v-model="house.constructionDate" type="number" id="houseConstructionDate">

            <label for="houseDetails">House Details:</label>
            <input v-model="house.details" type="text" id="houseDetails">

            <button type="submit">Save</button>
        </form>
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
            this.house = { ...selectedHouse };
            this.loadImage();
            console.log(this.house)
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

        saveChanges() {
            const houseIndex = this.$route.params.index;
            this.$store.dispatch('saveHouseChanges', { index: this.$route.params.index, house: this.house });

            // Redirect back to the house details page
            this.$router.push({ name: 'myhouse-details', params: { index: houseIndex } });
        }
    },
}

</script>