<template>
  <div class="col-2 border text-center">
    <div class="col-12">
      <div class="row time">15:00</div>
      <div class="row">ICON</div>
      <div class="row temperature">
        -1
        <sup>o</sup>C
      </div>
    </div>
    <div class="col-12 details">
      <div class="row">2.1 m/s</div>
      <div class="row">5 %</div>
      <div class="row">1 mm</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ForecastCity"
};
</script>

<style scoped>
.col {
  background-color: #ffffff;
}

.time {
  font-size: 13pt;
  color: #70757a;
}

.temperature {
  font-size: 15pt;
  color: #70757a;
}

.details {
  font-size: 10pt;
  color: #70757a;
  background-color: #e5f6fd;
}

.row {
  display: block;
}

.col-2 {
  padding-left: 0px;
  padding-right: 0px;
  background: #ffffff;
  flex: 0 0 19%;
  max-width: 19%;
}

.col-12 {
  padding: 0.5rem;
}
</style>