<template>
  <v-app>
    <div class="white headerSiteDiv py-2 widthAll fixed">
      <HeaderSite  />
    </div>
    
    <v-main>
      <v-row class="m-6">
        <v-col class="sidebarDiv heightAll white fixed">
          <SideBar />
        </v-col>
        <v-col class="mainPage">
          <router-view/>
        </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import HeaderSite from "@/components/HeaderSite.vue";
import SideBar from "@/components/SideBar.vue";

export default {
  name: "App",
  components: {
    HeaderSite,
    SideBar,
  },
  data: () => ({}),
};
</script>

<style>
/* hide scrollbar */
body::-webkit-scrollbar {
  display: none;
}

body {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

/* body background */
.v-main__wrap {
  height: 90vh !important;
  margin-left: 5%;
  margin-top: 53px;
}

.row {
  margin: 0 !important;
}

.col {
  padding: 0;
}

.formInput .v-text-field--outlined *{
  max-height: 45px !important;
}

.widthAll{
  width: 100%;
}

.heightAll{
  height: 100%;
}

.fixed{
  position: fixed;
}

.headerSiteDiv{
  top: 0;
  z-index: 3;
}

.relative{
  position: relative;
}

.absolute{
position: absolute;
}

.sidebarDiv{
  top: 20px;
  left: 0;
  z-index: 2;
  width: 5%;
  min-width: 64px;
}

.mainPage{
  width: 95%;
}

.v-main {
  background-color: #ededf5 !important;
}
</style>
