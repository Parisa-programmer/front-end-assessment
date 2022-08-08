<template>
  <div>
    <v-row class="mb-6">
      <h2 class="blue-grey--text text--darken-2">Products</h2>
      <v-spacer />
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-row>
    <v-row class="productTabel">
      <v-data-table
        :headers="headers"
        :items="products"
        :search="search"
        sort-by="name"
        class="elevation-1 widthAll"
      >
        <template v-slot:item.image="{ item }">
          <div class="p-2">
            <v-img
              :src="item.image"
              :alt="item.name"
              height="40px"
              width="40px"
            ></v-img>
          </div>
        </template>
        <template v-slot:top>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Are you sure you want to delete this item?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Cancel</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-row>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ProductsTabel",

  data: () => ({
    search: "",
    dialogDelete: false,
    headers: [
      { text: "", value: "image", sortable: false, filterable: false },
      { text: "NAME", value: "name", sortable: false, filterable: true },
      {
        text: "CATEGORY",
        value: "category",
        sortable: false,
        filterable: false,
        align: "center",
      },
      {
        text: "BRAND",
        value: "brand",
        sortable: false,
        filterable: false,
        align: "center",
      },
      {
        text: "PRICE",
        value: "price",
        sortable: false,
        filterable: false,
        align: "center",
      },
      {
        text: "DISCOUNT(%)",
        align: "center",
        sortable: false,
        value: "discount",
      },
      { text: "Actions", value: "actions", sortable: false },
    ],
    products: [],
    editedItem: {},
  }),

  watch: {
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  async created() {
    axios.get("https://dummyjson.com/products").then((response) => {
      this.products = response.data.products.map((x) => {
        return {
          image: x.images[0],
          name: x.title,
          category: x.category,
          brand: x.brand,
          price: x.price,
          discount: x.discountPercentage,
          id: x.id,
          description:x.description
        };
      });
    });
  },

  methods: {
    editItem(item) {
      this.editedItem = Object.assign({}, item);
      this.$emit("editClick", this.editedItem.id);
    },

    deleteItem(item) {
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    async deleteItemConfirm() {
      axios.delete("https://dummyjson.com/products/" + this.editedItem.id).then(
        this.$vs.notify({
          color: "success",
          title: "Done!",
          text: "The item deleted",
        }),
        this.closeDelete()
      );
    },

    closeDelete() {
      this.dialogDelete = false;
    },
  },
};
</script>

<style>
.formButtonsRow {
  bottom: 40px;
}

.productTabel .theme--light.v-data-table {
  background-color: unset !important;
  box-shadow: 0 0 0 white !important;
}

.productTabel th,
.productTabel .v-data-footer {
  color: #aaaaaa !important;
}

.productTabel tbody tr {
  background-color: #fff !important;
  margin-bottom: 20px !important;
}
</style>
