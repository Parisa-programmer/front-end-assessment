<template>
  <div class="about">
    <v-row class="px-4 pt-7">
      <v-col class="pr-4">
        <div class="white">
          <AddProduct :editItem="editItemId" />
        </div>
      </v-col>
      <v-col class="pl-4">
        <div class="">
          <ProductsTabel @editClick="editClick($event)" />
        </div>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import AddProduct from "@/components/AddProduct.vue";
import ProductsTabel from "@/components/ProductsTabel.vue";

export default {
  components: {
    AddProduct,
    ProductsTabel,
  },

  data: () => ({
    editItemId: null,
  }),
  methods: {
    editClick(event) {
      this.editItemId = event;
    },
  },
  created() {},
};
</script>
