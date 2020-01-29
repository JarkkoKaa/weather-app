<template>
  <b-alert variant="danger" show dismissible>
    <b-icon-alert-circle-fill></b-icon-alert-circle-fill>
    <b>Error</b>
    while getting data to {{ componentName }}, please try again later
  </b-alert>
</template>

<script>
import { BAlert, BIconAlertCircleFill } from "bootstrap-vue";
export default {
  name: "error",
  components: {
    BAlert,
    BIconAlertCircleFill
  },
  props: {
    componentName: String
  }
};
</script>

<style scoped>
</style>