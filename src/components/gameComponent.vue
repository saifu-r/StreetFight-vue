<template>
  <div class="container">
    <h1>Street Fighter</h1>

    <h2>Hunk's Heath</h2>
    <div class="bar-box">
      <div class="bar" :style="{ width: hunkHealth + '%' }"></div>
    </div>

    <h2>My Heath</h2>
    <div class="bar-box">
      <div class="bar" :style="{ width: myHealth + '%' }"></div>
    </div>

    <div class="resultBoard" v-if="winner">
      <h2 v-if="winner">Game Over !</h2>
      <h2 v-if="winner === 'iWin'">You Win !</h2>
      <h2 v-else-if="winner === 'hunkWin'">You Lose !</h2>
      <button @click="restartGame">Restart the Game </button>
      <!-- <h2 v-else>It's a Draw !</h2> -->
    </div>

    <div class="btn" v-else>
      <div class="action-btn"><button @click="myPunch">Punch</button></div>
      <div class="action-btn">
        <button
          @click="mykick"
          :disabled="currentRound % 4 !== 0 || currentRound === 0"
        >
          Kick
        </button>
      </div>
      <div class="action-btn"><button>Heal</button></div>
      <div class="action-btn"><button>Surrender</button></div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from "vue";

function generateRandomNumber(min: number, max: number) {
  return Math.floor(Math.random() * max - min) + min;
}

export default defineComponent({
  setup() {
    const hunkHealth = ref(100);
    const myHealth = ref(100);
    const currentRound = ref(0);
    const winner = ref("");

    const myPunch = () => {
      const randomNumber = generateRandomNumber(5, 15);
    //   if (hunkHealth.value <= 0) {
    //     winner.value = "iWin";
    //   }
      hunkHealth.value -= randomNumber;
      hunkPunch();
      currentRound.value++;
    };

    const hunkPunch = () => {
      const randomNumber = generateRandomNumber(8, 18);
    //   if (myHealth.value <= 0) {
    //     winner.value = "hunkWin";
    //   }
      myHealth.value -= randomNumber;
    };

    const mykick = () => {
      const randomNumber = generateRandomNumber(10, 20);
    //   if (hunkHealth.value <= 0) {
    //     winner.value = "iWin";
    //   }
      hunkHealth.value -= randomNumber;
      currentRound.value++;
      hunkPunch();
    };

    watch(hunkHealth, (value) =>{
        if(value<= 0){
            winner.value= 'iWin'
            hunkHealth.value= 0
        }
    })
    watch(myHealth, (value) =>{
        if(value<= 0){
            winner.value= 'hunkWin'
            myHealth.value= 0
        }
    })

    const restartGame= ()=>{
        hunkHealth.value= 100
        myHealth.value= 100
        currentRound.value= 0
        winner.value= ""
    }

    return {
      hunkHealth,
      myHealth,
      myPunch,
      hunkPunch,
      mykick,
      currentRound,
      winner,
      restartGame
    };
  },
});
</script>

<style scoped>
.container {
  width: 30%;
  height: auto;

  position: absolute;
  top: 10%;
  right: 35%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  border-radius: 31px;
  background: #e8e8e8;
  box-shadow: 6px 6px 12px #cecece, -6px -6px 12px #ffffff;
}
.bar-box {
  width: 80%;
  height: 1.5rem;
  background-color: rgb(255, 249, 249);
  border: 1px solid black;
}
.bar {
  width: 100%;
  height: 100%;
  background-color: green;
}
.btn {
  width: auto;
  margin: 20px;

  display: flex;
  flex-direction: column;
}
.action-btn button {
  margin: 5px;
  width: 100%;
  height: 30px;

  border-radius: 5px;
}
</style>