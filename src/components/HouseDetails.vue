<template>
    <div>
        <h1>house details</h1>
        <div v-if="house && house.length>0">
            <img :src="house[0].image">
            <h1 v-if="house[0].location">{{ house[0].location.city }}</h1>
            <h1 v-if="house[0].location">{{ house[0].location.street }}</h1>
            <h1 v-if="house[0].location">{{ house[0].location.houseNumber }}</h1>
            <h1>size:{{ house[0].size }}</h1>
            <h1>bed:{{ house[0].rooms.bedrooms }}</h1>
            <h1>bath:{{ house[0].rooms.bathrooms }}</h1>
        </div>
        <div v-else>
            <h1>Loading...</h1>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            house: [],
        }
    },

    async created() {
        const houseId = this.$route.params.id;

        const ApiKey = "_0KfzMBqomADtsj7brkVaX1iwRhLgIQN";


        const response= await fetch(`https://api.intern.d-tt.nl/api/houses/${houseId}`, {
            method: "GET",
            headers: {
                "X-Api-Key": ApiKey,
            }
        }).then((res) => res.json())
      .then((data) => {
        console.log(data);
        this.house = data;
      });

        

    }
}
</script>