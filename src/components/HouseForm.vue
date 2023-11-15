<template>
    <form @submit.prevent="submitForm" novalidate>
        <label for="streetName">House Street:</label>
        <input v-model="newHouse.street" type="text" id="streetName" required>

        <label for="houseNumber">House Number:</label>
        <input v-model="newHouse.number" type="text" id="houseNumber" required>

        <label for="houseAddition">House Addition:</label>
        <input v-model="newHouse.addition" type="text" id="houseAddition">

        <label for="housePostalCode">House Postal Code:</label>
        <input v-model="newHouse.postalCode" type="text" id="housePostalCode" required>

        <label for="houseCity">House City:</label>
        <input v-model="newHouse.city" type="text" id="houseCity" required>

        <label for="houseImage">House Image:</label>
        <input type="file" id="houseImage" @change="handleImageUpload" required>

        <label for="housePrice">House Price:</label>
        <input v-model="newHouse.price" type="text" id="housePrice" required>

        <label for="houseSize">House Size:</label>
        <input v-model="newHouse.size" type="text" id="houseSize" required>

        <label for="houseGarage">House Garage:</label>
        <select v-model="newHouse.garage" id="houseGarage" required>
            <option value="yes">Yes</option>
            <option value="no">No</option>
        </select>

        <label for="houseBedrooms">House Bedrooms:</label>
        <input v-model="newHouse.bedrooms" type="text" id="houseBedrooms" required>

        <label for="houseBathrooms">House Bathrooms:</label>
        <input v-model="newHouse.bathrooms" type="text" id="houseBathrooms" required>

        <label for="houseConstructionDate">House Construction Date:</label>
        <input v-model="newHouse.constructionDate" type="text" id="houseConstructionDate" required>

        <label for="houseDetails">House Details:</label>
        <input v-model="newHouse.details" type="text" id="houseDetails" required>

        <div v-if="!isFormValid" class="error-message">Please complete all required fields.</div>

        <button :disabled="!isFormValid" type="submit">Post</button>
    </form>
</template>

<script>


export default {
    data() {
        return {
            houses: [],
            houseImage: null,

            newHouse: {
                street: "",
                number: null,
                addition: "",
                postalCode: null,
                city: "",
                image: null,
                price: null,
                size: null,
                garage: "",
                bedrooms: null,
                bathrooms: null,
                constructionDate: null,
                details: ""
            }
        }
    },



    computed: {
        isFormValid() {

            // Check if all required fields are completed
            return this.newHouse.details.trim() !== ""
                && this.houseImage !== null
                && this.newHouse.street.trim() !== ""
                && this.newHouse.addition.trim() !== ""
                && this.newHouse.city.trim() !== ""
                && this.newHouse.garage.trim() !== ""
                && this.newHouse.constructionDate &&
                (
                    /^\d{2}\/\d{2}\/\d{4}$/.test(this.newHouse.constructionDate) ||
                    /^\d{2}-\d{2}-\d{4}$/.test(this.newHouse.constructionDate)
                )
                && (this.newHouse.price !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.number !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.postalCode !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.size !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.bedrooms !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.bathrooms !== null && !isNaN(this.newHouse.price))


                ;




        },
    },

    methods: {
        submitForm() {


            if (this.isFormValid) {
                this.$store.dispatch('addNewHouse', { newHouse: this.newHouse, image: this.houseImage });

                const index = this.$store.state.myListings.length - 1;

                // Redirect to the details page with the index as a parameter
                this.$router.push({ name: 'myhouse-details', params: { index } });
            }

        },

        handleImageUpload(event) {
            const file = event.target.files[0];

            // Check if a file was selected
            if (file) {
                // You can now use this file (image) as needed
                this.houseImage = file;
            }
        },


    }
}
</script>

<style>
.error-message {
    color: red;
    margin-top: 5px;
}
</style>