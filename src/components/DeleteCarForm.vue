<template>
  <div>
    <b-row>
      <h6 class="text-danger">Are you sure you want to delete this Car?</h6>
    </b-row>
    <b-row>
      <b-col>
        <b-button variant="danger" @click="removeCarFromData">Delete Car</b-button>
      </b-col>
      <b-col>
        <b-button variant="warning" @click="triggerClose">Close</b-button>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from "axios";

export default {
 
  props: {
    CarId: Number,
  },
  methods: {
    triggerClose() {
      this.$emit("closeDeleteModal");
    },
    removeCarFromData() {
      axios
        .delete(`http://localhost:3000/cars/${this.CarId}`)
        .then(() => {
          this.$emit("reloadDataTable");
          this.$emit("showDeleteAlert");
          this.$emit("closeDeleteModal");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>