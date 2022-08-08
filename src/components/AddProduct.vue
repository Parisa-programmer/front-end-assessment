<template>
  <div>
    <v-card class="pa-8" outlined>
      <div class="text--darken-1 indigo--text text-h4 mb-12">
        Create a New Product
      </div>
      <v-row>
        <v-col cols="8">
          <v-row class="formInput">
            <v-col>
              <label for="" class="text--darken-2 blue-grey--text">
                Product Name
                <span class="red--text">*</span>
              </label>
              <v-text-field
                class="mt-2 mb-4"
                v-model="name"
                :rules="nameRule"
                outlined
                clearable
              />
            </v-col>
          </v-row>
          <v-row class="formInput">
            <v-col cols="7">
              <label for="" class="text--darken-2 blue-grey--text">
                Prise
              </label>
              <v-text-field
                type="number"
                suffix="$"
                class="mt-2 mb-4"
                v-model="price"
                outlined
                :rules="priceRules"
              />
            </v-col>
            <v-col cols="5" class="pl-6">
              <label for="" class="text--darken-2 blue-grey--text">
                Discount
              </label>
              <v-text-field
                suffix="%"
                type="number"
                maxLength="3"
                :rules="discounrRules"
                class="mt-2 mb-4"
                v-model="discount"
                outlined
              />
            </v-col>
          </v-row>
          <v-row class="formInput">
            <v-col>
              <label for="" class="text--darken-2 blue-grey--text">
                Category
              </label>
              <v-select
                class="mt-2 mb-4"
                outlined
                v-model="category"
                :items="categoryItems"
              ></v-select>
            </v-col>
          </v-row>
          <v-row class="formInput">
            <v-col>
              <label for="" class="text--darken-2 blue-grey--text">
                Brand
              </label>
              <v-select
                class="mt-2 mb-4"
                outlined
                v-model="brand"
                :items="brandItems"
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <label for="" class="text--darken-2 blue-grey--text">
                Description
              </label>
              <v-textarea
                v-model="description"
                outlined
                class="mt-2 mb-4"
                name="input-7-4"
              ></v-textarea>
            </v-col>
          </v-row>
        </v-col>
        <v-col cols="4" class="relative">
          <v-row class="mt-4 justify-center">
            <div>
              <vs-upload
                v-model="image"
                limit="1"
                ref="objectImage"
                text="click to upload image"
                @delete="changeImage"
                :accept="acceptTypes"
              />
            </div>
          </v-row>
          <v-row class="justify-center">
            <p class="grey--text text--darken-1">
              file types:
              <b class="text--darken-3 indigo--text">.jpg, .png, .pdf</b>
              <br />
              File size:<b class="text--darken-3 indigo--text">2mb</b>
            </p>
          </v-row>
          <v-row class="formButtonsRow absolute justify-center widthAll">
            <v-btn
              text
              depressed
              color="indigo"
              class="mr-3"
              bold
              @click="clearFields"
            >
              {{ editMood ? "Cancell" : "Clear" }}
            </v-btn>
            <v-btn depressed color="indigo" dark @click="submit">
              {{ editMood ? "Update" : "Submit" }}
            </v-btn>
          </v-row>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script>
// import { async } from 'q';
import axios from "axios";

export default {
  name: "AddProduct",
  props: {
    editItem: {
      type: Number,
      require: false,
    },
  },
  data: () => ({
    name: "",
    editMood: false,
    price: 0,
    discount: 0,
    categoryItems: [],
    category: "",
    brand: "",
    brandItems: ["brand 1", "brand 2", "brand 3", "brand 4"],
    description: "",
    discounrRules: [
      (v) => v <= 100 || "must be less than 100",
      (v) => v >= 0 || "must be more than 0",
    ],
    priceRules: [(v) => v >= 0 || "must be more than 0"],
    nameRule: [(v) => !!v || "name is required!"],
    editObject: {},
    image: "",
    acceptTypes: [".jpg,.png,.pdf"],
  }),
  methods: {
    changeImage() {
      this.$refs.objectImage.srcs = [];
    },
    clearFields() {
      this.name = "";
      this.price = 0;
      this.discount = 0;
      this.category = "";
      this.brand = "";
      this.description = "";
      this.editMood = false;
      this.$refs.objectImage.srcs = [];
    },
    async submit() {
      if (
        this.name.length != 0 &&
        this.price.length != 0 &&
        this.price >= 0 &&
        this.discount.length != 0 &&
        this.discount >= 0 &&
        this.discount < 100
      ) {
        if (this.$refs.objectImage.srcs.length) {
          var lastImage = this.$refs.objectImage.srcs.length - 1;
          const img = this.$refs.objectImage.srcs[lastImage].src;
          const buffer = Buffer.from(img.substring(img.indexOf(",") + 1));
          if (buffer.length / 1e6 > 2) {
            this.$vs.notify({
              color: "danger",
              title: "Error!",
              text: "max image size must be 2mb.",
            });
          } else {
            var sendObject = {
              name: this.name,
              price: this.price,
              discount: this.discount,
              category: this.category,
              brand: this.brand,
              description: this.description,
            };
            if (this.editMood) {
              axios
                .put(
                  "https://dummyjson.com/products/" + this.editItem,
                  sendObject
                )
                .then((response) => {
                  console.log(response);
                  this.$vs.notify({
                    color: "success",
                    title: "Done!",
                    text: "The product updated.",
                  });
                  this.clearFields();
                });
            } else {
              axios
                .post("https://dummyjson.com/products/add", sendObject)
                .then((response) => {
                   console.log(response);
                  this.$vs.notify({
                    color: "success",
                    title: "Done!",
                    text: "The product created.",
                  });
                  this.clearFields();
                });
            }
          }
        } else {
          this.$vs.notify({
            color: "danger",
            title: "Error!",
            text: "Choose an image.",
          });
        }
      } else {
        this.$vs.notify({
          color: "danger",
          title: "Error!",
          text: "Please complate the fields currectly.",
        });
      }
    },
  },
  async created() {
    axios.get("https://dummyjson.com/products/categories").then((response) => {
      this.categoryItems = response.data;
    });
  },
  watch: {
     async editItem() {
      axios
        .get("https://dummyjson.com/products/" + this.editItem)
        .then((response) => {
          var res = response.data;
          this.editMood = true;
          this.name = res.title;
          this.price = res.price;
          this.discount = res.discountPercentage;
          this.category = res.category;
          this.brand = res.brand;
          this.description = res.description;
          this.$refs.objectImage.srcs = [];
          this.$refs.objectImage.srcs.push({
            error: false,
            orientation: "w",
            percent: null,
            remove: null,
            type: "image",
            src: res.images[0],
          });
        });
    },
  },
  computed: {},
  mounted() {},
};
</script>

<style>
.formButtonsRow {
  bottom: 40px;
}

.formInput .v-input__control {
  display: block !important;
}

.formInput .v-text-field.v-text-field--enclosed .v-text-field__details {
  margin-top: -21px;
}
</style>
