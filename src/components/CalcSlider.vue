<template>
  <div id="current-stars-div">
    <label for="current-stars">{{ title }}</label>
    <input
      @change="updateMaxCurrentShards"
      type="range"
      min="0"
      max="7"
      v-model="currentStars"
      class="slider"
      name="current-stars"
      id="current-stars"
    />
    <div id="current-stars-display">{{ displayValue }}</div>
  </div>
</template>

<script>
export default {
  name: "CalcSlider",
  props: {
    title: String,
    displayValue: Number,
  },
};
</script>

<style scoped>
slidecontainer {
  width: 100%; /* Width of the outside container */
}

/* The slider itself */
.slider {
  -webkit-appearance: none; /* Override default CSS styles */
  appearance: none;
  width: 250px;
  height: 15px;
  border-radius: 25px;
  background: #e5e7e6;
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
  display: block;
}

/* Mouse-over effects */
.slider:hover {
  opacity: 1;
}

/* The slider handle (use -webkit- (Chrome, Opera, Safari, Edge) and -moz- (Firefox) to override default look) */
.slider::-webkit-slider-thumb {
  -webkit-appearance: none; /* Override default look */
  appearance: none;
  width: 15px;
  height: 15px;
  background: #b80c09;
  cursor: pointer;
  border-radius: 50%;
}

.slider::-moz-range-thumb {
  width: 15px;
  height: 15px;
  background: #b80c09;
  cursor: pointer;
}
</style>
