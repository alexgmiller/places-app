<script>
import axios from "axios";
export default {
  data: function () {
    return {
      places: [],
      currentPlace: {},
      newPlaceName: "",
      newPlaceAddress: "",
    };
  },
  created: function () {},
  methods: {
    indexPlaces: function () {
      axios.get("/places").then((response) => {
        console.log("places index", response);
        this.places = response.data;
      });
    },
    createPlace: function () {
      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress,
      };
      axios
        .place("/places", params)
        .then((response) => {
          this.places.push(response.data);
          this.newPlaceName = "";
          this.newPlaceAddress = "";
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    showPlace: function (place) {
      if (place === this.currentplace) {
        this.currentplace = {};
      } else {
        this.currentplace = place;
      }
    },
    updatePlace: function (place) {
      var params = {
        title: place.title,
        image: place.image,
        body: place.body,
      };
      axios
        .patch("/places/" + place.id, params)
        .then((response) => {
          place.name = response.data.name;
          place.address = response.data.address;
          this.currentPlace = {};
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    destroyPlace: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log(response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Place</h1>
    <input type="text" v-model="newPlaceName" />
    <input type="text" v-model="newPlaceAddress" />
    <button @click="createPlace()">Create</button>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>Name: {{ place.name }}</h2>
      <h2>Address: {{ place.address }}</h2>
      <button @click="showPlace(place)">more info</button>
    </div>
    <h1>All Places</h1>
    <div v-if="place === currentPlace">
      <h2>
        {{ place.name }}
      </h2>
      <h2>
        {{ place.address }}
      </h2>
      <div>
        <h4>Edit place</h4>
        Name:
        <input type="text" v-model="place.name" />
        Address:
        <input type="text" v-model="place.address" />
        <button v-on:click="updatePlace(place)">Update Place</button>
        <button v-on:click="destroyPlace(place)">Destroy Place</button>
      </div>
    </div>
  </div>
</template>

<style></style>
