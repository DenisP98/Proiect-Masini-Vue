<template>
  <b-form>
    <b-form-group label="Car Brand">
      <b-form-input
        id="car_brand"
        type="text"
        v-model="Car.car_brand"
      ></b-form-input>
    </b-form-group>

    <b-form-group label="Car Model">
      <b-form-input
        id="car-model"
        type="text"
        v-model="Car.car_model"
      ></b-form-input>
    </b-form-group>

    <b-form-group label="Car Price">
      <b-form-input
        id="car-price"
        type="text"
        v-model="Car.car_price"
      ></b-form-input>
    </b-form-group>

    <b-form-group label="Car Color">
      <b-form-input
        id="car-color"
        type="text"
        v-model="Car.car_color"
      ></b-form-input>
    </b-form-group>

    <b-button variant="primary" class="px-5" @click="updateCar">Update Car</b-button>

    <b-button variant="warning" class="px-5" @click="triggerClose">Close</b-button>

  </b-form>
</template>

<script>
import axios from "axios";

export default {
  
  props: {
    CarId: Number,
  },
  data() {
    return {
      Car: {},
    };
  },
  mounted() {
    this.getCarByID();
  },
  methods: {
    triggerClose() {
      this.$emit("closeEditModal");
    },
    getCarByID() {
      axios
        .get(`http://localhost:3000/cars/${this.CarId}`)
        .then((response) => {
          this.Car = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateCar() {
      axios
        .put(`http://localhost:3000/cars/${this.CarId}`, this.Car)
        .then((response) => {
          console.log(response.data);
          this.$emit("closeEditModal");
          this.$emit("reloadDataTable");
          this.$emit("showSuccessAlert");
        })
        .catch((error) => {
          console.log(error);
        });
    },
    
  },
};
</script>