<template>
  <section id="grid">
    <button
      :disabled="index > 0 && index > firstInCompleteWorkoutIndex"
      @click="() => handleSelectWorkout(index)"
      v-for="(workout, index) in Object.keys(workoutProgram)"
      :key="index"
      class="card-button plan-card"
    >
      <div>
        <p>Day {{ index < 9 ? "0" + (index + 1) : index + 1 }}</p>
        <i class="fa-solid fa-dumbbell" v-if="index % 3 == 0"></i>
        <i class="fa-solid fa-weight-hanging" v-if="index % 3 == 1"></i>
        <i class="fa-solid fa-bolt" v-if="index % 3 == 2"></i>
      </div>

      <h3>{{ workoutTypes[index % 3] }}</h3>

      <div class="lock" v-if="index > 0 && index > firstInCompleteWorkoutIndex">
        <i class="fa-solid fa-lock"></i>
      </div>
    </button>

    <button
      :disabled="firstInCompleteWorkoutIndex != -1"
      @click="handleResetPlan"
      class="card-button plan-card-reset"
    >
      <p>Reset</p>
      <i class="fa-solid fa-rotate-left"></i>
    </button>
  </section>
</template>

<script setup>
import { workoutProgram } from "../utlis";
import { defineProps } from "vue";

defineProps({
  handleSelectWorkout: Function,
  firstInCompleteWorkoutIndex: Number,
  handleResetPlan: Function,
});
const workoutTypes = ["Push", "Pull", "Legs"];
</script>

<style scoped>
#grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}

#grid button {
  width: 100%;
  position: relative;
}

.lock {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 40px;
  background: "#fff";
}

#grid button:disabled {
  box-shadow: none;
  cursor: not-allowed;
}

.plan-card {
  display: flex;
  flex-direction: column;
}

.plan-card-reset {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.plan-card div {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
}

/* Küçük ekranlar için örnek 
@media (max-width: 428px) {
  #grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}
*/
/* Orta ekranlar için örnek 
@media (min-width: 429px) and (max-width: 758px) {
  #grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}
*/
/* Geniş ekranlar için 
@media (min-width: 759px) {
  #grid {
    grid-template-columns: repeat(5, minmax(0, 1fr));
  }
}
*/

.plan-card div p {
  text-align: left;
}

@media (min-width: 640px) {
  #grid {
    grid-template-columns: repeat(5, minmax(0, 1fr));
  }
}
</style>
