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

    <div class="result-board" v-if="winner">
      <h2 v-if="winner">Game Over !</h2>
      <h2 v-if="winner === 'iWin'" style="color: green">You Win !</h2>
      <h2 v-else-if="winner === 'hunkWin'" style="color: red">You Lose !</h2>
      <div class="action-btn">
        <button @click="restartGame">Restart the Game</button>
      </div>
      <!-- <h2 v-else>It's a Draw !</h2> -->
    </div>

    <div class="btn" v-else>
      <div class="action-btn"><button @click="myPunch">Punch</button></div>
      <div class="action-btn">
        <button
          @click="mykick"
          :disabled="currentRound % 3 !== 0 || currentRound === 0"
        >
          Kick
        </button>
      </div>
      <div class="action-btn">
        <button
          @click="healHealth"
          :disabled="countHeal>=2 || currentRound === 0"
        >
          Heal
        </button>
      </div>
      <div class="action-btn"><button @click="surrenderGame">Surrender</button></div>
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
    const countHeal= ref(0)

    const myPunch = () => {
      const randomNumber = generateRandomNumber(5, 12);
      //   if (hunkHealth.value <= 0) {
      //     winner.value = "iWin";
      //   }
      hunkHealth.value -= randomNumber;
      hunkPunch();
      currentRound.value++;
    };

    const hunkPunch = () => {
      const randomNumber = generateRandomNumber(5, 15);
      //   if (myHealth.value <= 0) {
      //     winner.value = "hunkWin";
      //   }
      myHealth.value -= randomNumber;
    };

    const mykick = () => {
      const randomNumber = generateRandomNumber(15, 20);
      //   if (hunkHealth.value <= 0) {
      //     winner.value = "iWin";
      //   }
      hunkHealth.value -= randomNumber;
      currentRound.value++;
      hunkPunch();
    };

    const healHealth = () => {
      const randomNumber = generateRandomNumber(10, 20);
      myHealth.value += randomNumber;
      if (myHealth.value > 100) {
        myHealth.value = 100;
      }

      currentRound.value++;
      hunkPunch();
      countHeal.value++
    };

    watch(hunkHealth, (value) => {
      if (value <= 0) {
        winner.value = "iWin";
        hunkHealth.value = 0;
      }
    });
    watch(myHealth, (value) => {
      if (value <= 0) {
        winner.value = "hunkWin";
        myHealth.value = 0;
      }
    });

    const restartGame = () => {
      hunkHealth.value = 100;
      myHealth.value = 100;
      currentRound.value = 0;
      winner.value = "";
      countHeal.value= 0
    };

    const surrenderGame= ()=>{
        winner.value= 'hunkWin'
    }

    return {
      hunkHealth,
      myHealth,
      myPunch,
      hunkPunch,
      mykick,
      currentRound,
      winner,
      restartGame,
      healHealth,
      countHeal,
      surrenderGame,
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

.result-board {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
}
.result-board h2 {
  margin: 10px;
}

.result-board button {
  margin-bottom: 40px;
}
</style>