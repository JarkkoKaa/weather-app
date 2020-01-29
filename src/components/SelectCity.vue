<template>
  <b-container fluid class="container-select marginbottom">
    <select v-model="selected" @change="getSelected" class="form-control">
      <option v-for="option in options" :value="option.value" :key="option.value">{{ option.name }}</option>
    </select>
  </b-container>
</template>

<script>
import { BContainer } from "bootstrap-vue";

export default {
  name: "SelectCity",
  components: {
    BContainer
  },
  data() {
    return {
      selected: "all_cities"
    };
  },
  props: {
    options: Array
  },
  methods: {
    getSelected() {
      this.$emit("selectedCity", this.selected);
    }
  }
};
</script>

<style scoped>
.container-select {
  padding-left: 0px;
  padding-right: 0px;
  margin-bottom: 1.25rem;
}

select {
  margin-bottom: 0px;
  height: 3rem;
  color: #262626;
  font-size: 13pt;
}
</style>
