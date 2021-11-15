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
     
    
        <b-button variant="primary" class="px-5" @click="addNewCar">Add Car</b-button>
     
        <b-button variant="warning" class="px-5" @click="triggerClose">Close</b-button>
    
  </b-form>
</template>

<script>
import axios from "axios";

export default {
  
  data() {
    return {
      Car: {},
    };
  },
  methods: {
    triggerClose() {
      this.$emit("closeCreateModal");
    },
    addNewCar() {
      axios
        .post("http://localhost:3000/cars/", this.Car)
        .then((response) => {
          console.log(response.data);
          this.$emit("closeCreateModal");
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

