<template>
  <v-bottom-navigation
    height="unset"
    class="pt-9 flex-column menuSideBar"
    :value="value"
    color="primary"
  >
    <v-btn v-for="(items, i) in icones" :key="i" class="white py-8 px-0">
      <router-link :to="'/' + items.link">
        <v-icon size="34">{{ items.icon }}</v-icon>
      </router-link>
    </v-btn>
  </v-bottom-navigation>
</template>
<script>
export default {
  name: "SideBar",
  data() {
    return {
      value: 0,
      icones: [
        { icon: "mdi-image", link: "image" },
        { icon: "mdi-help-box", link: "help" },
        { icon: "mdi-view-dashboard", link: "dashboard" },
        { icon: "mdi-cog ", link: "setting" },
      ],
    };
  },
};
</script>

<style>
.v-item-group.v-bottom-navigation {
  box-shadow: 0px 0px 0px 0px white !important;
}

.menuSideBar a {
  color: unset !important;
}
</style>
