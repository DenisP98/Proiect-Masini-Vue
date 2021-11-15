<template>
  <div>
   

    <b-row>
      <b-Card>
        <b-row>
          <b-table
            striped
            hover
            :items="items"
            :fields="fields"
            class="text-center"
          >
            <template #cell(actions)="data">
              <b-row>
                <b-col>
                  <b-icon-pencil-square
                    class="action-item"
                    variant="primary"
                    @click="getRowData(data.item.id)"
                  ></b-icon-pencil-square>
                </b-col>
                <b-col>
                  <b-icon-trash-fill
                    class="action-item"
                    variant="danger"
                    @click="showDeleteModal(data.item.id)"
                  ></b-icon-trash-fill>
                </b-col>
              </b-row>
            </template>
          </b-table>
        </b-row>

        <b-col>
          <b-button variant="primary" id="show-btn" @click="showCreateModal">
            <b-icon-plus class="text-white"></b-icon-plus>
            <span class="h6 text-white">New Car</span>
          </b-button>
        </b-col>
      </b-Card>
    </b-row>
<br>

     <b-row>
      <b-alert v-model="showSuccessAlert" variant="success" dismissible>
        {{ alertMessage }}
      </b-alert>
    </b-row>

    <!-- Modal adaugare masina -->
    <b-modal ref="create-car-modal" size="xl" hide-footer title="New Car">
      <create-car-form
        @closeCreateModal="closeCreateModal"
        @reloadDataTable="getCarData"
        @showSuccessAlert="showAlertCreate"
      ></create-car-form>
    </b-modal>

    <!-- Modal update masina -->
    <b-modal ref="edit-car-modal" size="xl" hide-footer title="Edit Car">
      <edit-car-form
        @closeEditModal="closeEditModal"
        @reloadDataTable="getCarData"
        @showSuccessAlert="showAlertUpdate"
        :CarId="CarId"
      ></edit-car-form>
    </b-modal>

    <!-- Modal stergere masina -->
    <b-modal
      ref="delete-car-modal"
      size="md"
      hide-footer
      title="Confirm Deletion"
    >
      <delete-car-form
        @closeDeleteModal="closeDeleteModal"
        @reloadDataTable="getCarData"
        @showDeleteAlert="showDeleteSuccessModal"
        :CarId="CarId"
      ></delete-car-form>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
import CreateCarForm from "@/components/CreateCarForm.vue";
import EditCarForm from "@/components/EditCarForm.vue";
import DeleteCarForm from "@/components/DeleteCarForm.vue";

export default {
  components: {
    CreateCarForm,
    EditCarForm,
    DeleteCarForm,
  },
  data() {
    return {
      fields: [
        {
          key: "car_brand",
          label: "Brand",
        },
        {
          key: "car_model",
          label: "Model",
        },
        {
          key: "car_price",
          label: "Price",
        },
        {
          key: "car_color",
          label: "Color",
        },
        "actions",
      ],
      items: [],
      CarId: 0,
      showSuccessAlert: false,
      alertMessage: ""
    };
  },
  mounted() {
    this.getCarData();
  },
  methods: {
    showCreateModal() {
      this.$refs["create-car-modal"].show();
    },
    closeCreateModal() {
      this.$refs["create-car-modal"].hide();
    },
    getCarData() {
      axios
        .get("http://localhost:3000/cars/")
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getRowData(id) {
      this.$refs["edit-car-modal"].show();
      this.CarId = id;
    },
    closeEditModal() {
      this.$refs["edit-car-modal"].hide();
    },
    showDeleteModal(id) {
      this.$refs["delete-car-modal"].show();
      this.CarId = id;
    },
    closeDeleteModal() {
      this.$refs["delete-car-modal"].hide();
    },
    showDeleteSuccessModal() {
      this.showSuccessAlert = true;
      this.alertMessage = "Car instance was deleted successfully!";
    },
    showAlertCreate() {
      this.showSuccessAlert = true;
      this.alertMessage = "New Car instance created successfully!";
    },
    showAlertUpdate() {
      this.showSuccessAlert = true;
      this.alertMessage = "The information was updated successfully.";
    },
  },
};
</script>

<style>
.action-item:hover {
  cursor: pointer;
}
</style>
