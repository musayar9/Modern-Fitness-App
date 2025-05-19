<template>
  <section id="dashboard">
    <div class="card tip-container">
      <h2>Welcome Smoldier</h2>
      <div>
        <p class="tip">
          <strong>Daily Tip</strong><br />
          {{ todaysFact }}
        </p>
      </div>
      <button
        @click="
          () =>
            handleSelectWorkout(
              firstInCompleteWorkoutIndex < 0 ? 0 : firstInCompleteWorkoutIndex
            )
        "
      >
        Start Workout &rarr;
      </button>
    </div>

    <Grid :handleResetPlan="handleResetPlan" :handleSelectWorkout="handleSelectWorkout" :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex" />
  </section>
</template>

<script setup>
import { gymHealthFacts } from "../../utlis";
import Grid from "../Grid.vue";
import { defineProps } from "vue";

defineProps({
  handleSelectWorkout: Function,
  firstInCompleteWorkoutIndex: Number,
  handleResetPlan:Function
});

// generate a random whole integer number between 0 and array length -1
const randomNumber = Math.floor(Math.random() * gymHealthFacts.length);
const todaysFact = gymHealthFacts[randomNumber];
console.log(todaysFact);
</script>

<style scoped>
.tip-container,
.tip-container div,
#dashboard {
  display: flex;
}

.tip-container,
#dashboard {
  flex-direction: column;
}

#dashboard {
  gap: 2rem;
}

.tip-container {
  gap: 0.5rem;
}

@media (min-width: 640px) {
  .tip-container {
    gap: 1rem;
  }
}
</style>
