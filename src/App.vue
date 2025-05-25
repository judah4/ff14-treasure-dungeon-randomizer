<template>
  <h1>Final Fantasy XIV Treasure Dungeon Door Randomizer!</h1>
 <p>
    Pick a door, any door! Take the decisions out of running treasure dungeons with the power of randomness!
  </p>
<template v-if="!calculating">
 
  <h2>Left or Right?</h2>
  <div>
    <label><input type="checkbox" v-model="alwaysLeft"> Apply Merelle Algorithm</label>
  </div>
  <br/>
<div>
  <button @click="calc()" > Calculate </button>
</div>
</template>
<template v-else>
<transition >
  <div v-if="calcStep>=0">
    Calculating base decision...
  </div>
</transition>

<transition >
  <div v-if="calcStep>=1">
    Grabbing background radiation...
  </div>
</transition>

<transition >
  <div v-if="calcStep>=2">
    Checking local tide and wave height...
  </div>
</transition>
  
<transition >
  <div v-if="calcStep>=3">
    Finding prime numbers...
  </div>
</transition>
  
<transition>
  <div v-if="calcStep>=4 && alwaysLeft">
    Applying Josh Algorithm...
  </div>
</transition>
<br/>
<transition>
  <div v-if="calcDone">
    <h2>And the result is!</h2>
    <h3 v-if="left">
      Left
    </h3>
    <h3 v-else>
      Right
    </h3>
    <br/>
    <div>
      <button @click="reroll()" > Reroll </button>
    </div>
    <div>
      <button @click="reset()" > Settings </button>
    </div>
  </div>

</transition>
</template>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      alwaysLeft: true,
      calculating: false,
      calcStep: 0,
      calcDone: false,
      left: false,
    }
  },
  methods: {
    calc: function() {
      this.calculating = true;
      
      this.CalcTimer();

    },
    CalcTimer: function() {
      var self = this;
      setTimeout(function(){ 
        var maxStep = 4;
        if(self.alwaysLeft) {
          maxStep = 5;
        }
        self.calcStep++;

        self.left = self.getRandomInt(2);
        if(self.calcStep >= 4 && self.alwaysLeft) {
          self.left = true;
        }

        if(self.calcStep >= maxStep) {
          self.calcDone = true;
        } 

        if(self.calcDone == false) {
          self.CalcTimer();
        }
        
        }, 760 + (74 * self.calcStep));
    },
    reset: function() {
      this.calculating = false;
      this.calcStep = 0;
      this.calcDone = false;
    },
    reroll: function() {
      this.reset();
      this.calc();
    },
    getRandomInt: function (max) {
      return Math.floor(Math.random() * max);
  }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}


</style>
