<template>
  <vs-row class="px-2">
    <v-img
      class="shrink mr-2 bg-white"
      contain
      src="../assets/shopping.png"
      transition="scale-transition"
      width="70"
    />
    <v-spacer></v-spacer>
    <vs-avatar size="30px" />
    <vs-select class="selectExample" v-model="select1" color="#fb9cc8">
      <vs-select-item
        class="align-right red--text darken-2--text"
        :key="index"
        :value="item.value"
        :text="item.text"
        v-for="(item, index) in options2"
      />
    </vs-select>
  </vs-row>
</template>

<script>
export default {
  name: "HeaderSite",

  data: () => ({
    select1: 1,
    options2: [
      { text: "Admin", value: 1 },
      { text: "User", value: 2 },
    ],
  }),
  created() {
    
  },
};
</script>

<style>
.headerSiteDiv .vs-select--input {
  color: #aeaeae !important;
}

.selectExample {
  width: 90px !important;
}

.selectExample .vs-select--input {
  border: 0 !important;
  text-align: right;
  padding-right: 32px !important;
  font-size: 15px !important;
}
</style>
