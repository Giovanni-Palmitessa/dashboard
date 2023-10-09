<script>
import { initFlowbite } from "flowbite";
import AppDashboard from "./components/AppDashboard.vue";
export default {
  components: {
    AppDashboard,
  },
  mounted() {
    initFlowbite();
  },
};
</script>

<template>
  <AppDashboard />
</template>

<style lang="scss"></style>
