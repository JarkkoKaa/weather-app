<template>
  <div class="container">
    <select v-model="selected" @change="getSelected" class="form-control">
      <option
        v-for="option in options"
        :value="option.value"
        :key="option.value"
        >{{ option.name }}</option
      >
    </select>
  </div>
</template>

<script>
export default {
  name: "SelectCity",
  data() {
    return {
      selected: "634964"
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
.container {
  padding-left: 0px;
  padding-right: 0px;
  margin-bottom: 2vh;
}

select {
  margin-bottom: 0px;
}
</style>
