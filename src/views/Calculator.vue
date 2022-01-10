<template>
  <div class="calculator">
    <div id="form-container">
      <div id="current-stars-div">
        <label for="current-stars">Current Stars</label>
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
        <div id="current-stars-display">{{ currentStars }}</div>
      </div>

      <div id="current-shards-div">
        <label for="current-shards">Current Shards</label>
        <input
          type="range"
          min="0"
          v-bind:max="maxCurrentShards"
          v-model="currentShards"
          class="slider"
          name="current-shards"
          id="current-shards"
        />
        <div id="current-shards-display">{{ currentShards }}</div>
      </div>

      <div
        id="accelerated-character-div"
        style="
          display: flex;
          flex-direction: column;
          align-items: center;
          padding: 0px 0px 5px 0px;
        "
      >
        <label for="accelerated-character">Accelerated Character</label>
        <input
          type="checkbox"
          v-model="acceleratedCharacter"
          class="checkbox"
          name="accelerated-character"
          id="accelerated-character"
          style="display: block; text-align: center"
        />
      </div>

      <div id="character-node-div">
        <label for="character-nodes">Character Nodes</label>
        <input
          type="range"
          min="0"
          max="2"
          v-model="characterNodes"
          @change="calculateMaxNodeRefreshes"
          class="slider"
          name="character-nodes"
          id="character-nodes"
        />
        <div id="character-nodes-display">{{ characterNodes }}</div>
      </div>

      <div id="fleet-node-div">
        <label for="fleet-nodes">Fleet Nodes</label>
        <input
          type="range"
          min="0"
          max="2"
          v-model="fleetNodes"
          @change="calculateMaxNodeRefreshes"
          class="slider"
          name="fleet-nodes"
          id="fleet-nodes"
        />
        <div id="fleet-nodes-display">{{ fleetNodes }}</div>
      </div>

      <div id="node-refreshes-div">
        <label for="node-refreshes">Dailey Node Refreshes</label>
        <input
          type="range"
          min="0"
          v-bind:max="maxNodeRefreshes"
          v-model="nodeRefreshes"
          class="slider"
          name="node-refreshes"
          id="node-refreshes"
        />
        <div id="node-refreshes-display">{{ nodeRefreshes }}</div>
      </div>

      <div id="results-button-div">
        <button id="results-button" @click="calculateDaysToFarm">
          Estimate Farm
        </button>
      </div>

      <div id="results-div" class="hidden">
        <div id="results">{{ daysToFarm }} days to seven star!</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  props: {},
  components: {},
  data: function () {
    return {
      currentStars: 0,
      starShards: 0,
      currentShards: 0,
      maxCurrentShards: 330,
      shardsNeeded: 330,
      testResults: 0,
      characterNodes: 0,
      fleetNodes: 0,
      nodeRefreshes: 0,
      maxNodeRefreshes: 0,
      maxAttempts: 0,
      dropRate: 0.33,
      acceleratedCharacter: false,
      daysToFarm: 0,
    };
  },
  methods: {
    testResultsFunction: function () {
      this.testResults =
        parseInt(this.currentStars) * parseInt(this.currentShards);
    },
    calculateMaxNodeRefreshes: function () {
      // each node can be refreshed 7 times
      // max refreshes = total nodes * 7
      this.maxNodeRefreshes =
        (parseInt(this.characterNodes) + parseInt(this.fleetNodes)) * 7;
      // reset node refreshes to 0
      this.nodeRefreshes = 0;
    },
    calculateMaxAttempts: function () {
      this.maxAttempts =
        (parseInt(this.characterNodes) +
          parseInt(this.fleetNodes) +
          parseInt(this.nodeRefreshes)) *
        5;
    },
    calculateStarShards: function () {
      if (this.currentStars == 0) {
        this.starShards = 0;
      } else if (this.currentStars == 1) {
        this.starShards = 10;
      } else if (this.currentStars == 2) {
        this.starShards = 25;
      } else if (this.currentStars == 3) {
        this.starShards = 50;
      } else if (this.currentStars == 4) {
        this.starShards = 80;
      } else if (this.currentStars == 5) {
        this.starShards = 145;
      } else if (this.currentStars == 6) {
        this.starShards = 230;
      } else if (this.currentStars == 7) {
        this.starShards = 330;
      }
    },
    updateMaxCurrentShards: function () {
      if (this.currentStars == 0) {
        this.maxCurrentShards = 330;
      } else if (this.currentStars == 1) {
        this.maxCurrentShards = 320;
      } else if (this.currentStars == 2) {
        this.maxCurrentShards = 305;
      } else if (this.currentStars == 3) {
        this.maxCurrentShards = 280;
      } else if (this.currentStars == 4) {
        this.maxCurrentShards = 250;
      } else if (this.currentStars == 5) {
        this.maxCurrentShards = 185;
      } else if (this.currentStars == 6) {
        this.maxCurrentShards = 100;
      } else if (this.currentStars == 7) {
        this.maxCurrentShards = 0;
      }
      this.currentShards = 0;
    },
    calculateShardsNeeded: function () {
      this.calculateStarShards();
      this.shardsNeeded =
        330 - (parseInt(this.starShards) + parseInt(this.currentShards));
      if (this.shardsNeeded < 0) {
        this.shardsNeeded = 0;
      } else if (this.shardsNeeded > 330) {
        this.shardsNeeded = 330;
      }
    },
    calculateDaysToFarm: function () {
      this.calculateMaxAttempts();
      this.calculateShardsNeeded();
      if (this.maxAttempts > 0) {
        this.daysToFarm =
          parseInt(this.shardsNeeded) /
          parseFloat(this.dropRate) /
          parseInt(this.maxAttempts);
        if (this.acceleratedCharacter) {
          this.daysToFarm = this.daysToFarm / 2;
        }
        this.daysToFarm = this.daysToFarm.toFixed(1);
        if (this.daysToFarm < 1) {
          this.daysToFarm = 1;
        }
        // reveal results-div
        document.querySelector("#results-div").classList.remove("hidden");
      }
      this.testLogValues();
    },
    testLogValues: function () {
      // log all values for testing
      console.log({
        currentStars: this.currentStars,
        starShards: this.starShards,
        currentShards: this.currentShards,
        shardsNeeded: this.shardsNeeded,
        testResults: this.testResults,
        characterNodes: this.characterNodes,
        fleetNodes: this.fleetNodes,
        nodeRefreshes: this.nodeRefreshes,
        maxNodeRefreshes: this.maxNodeRefreshes,
        maxAttempts: this.maxAttempts,
        dropRate: this.dropRate,
        acceleratedCharacter: this.acceleratedCharacter,
        daysToFarm: this.daysToFarm,
      });
    },
  },
};
</script>

<style scoped>
.hidden {
  visibility: hidden;
}

#results-button-div {
  padding: 30px 0px;
}

#results-button {
  background-color: #b80c09;
  color: #e5e7e6;
  font-family: StarJedi, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 250px;
  height: 50px;
  border: solid 2px #e5e7e6;
  border-radius: 25px;
  cursor: pointer;
}

.calculator {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
}

#form-container {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  max-width: 50vw;
}

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
