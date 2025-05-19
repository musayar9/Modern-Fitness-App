<script setup>
import { computed, onMounted, reactive, ref } from "vue";

import Layout from "./components/layout/Layout.vue";
import Dashboard from "./components/pages/Dashboard.vue";
import Welcome from "./components/pages/Welcome.vue";
import Workout from "./components/pages/Workout.vue";
import { workoutProgram } from "./utlis";

const defaultData = {};
for (let workoutIdx in workoutProgram) {
  const workoutData = workoutProgram[workoutIdx];
  // create o for loop where we iterate over every workout
  defaultData[workoutIdx] = {}; //initialize the workout data obj

  // nested loop to over every exercise within a workout, and initialize it's input value to an empty string
  //
  // bir antrenman içindeki her egzersizi kapsayan ve giriş değerini boş bir dizeye başlatan iç içe döngü

  for (let e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = "";
  }
}
console.log("defualt data", defaultData);
const selectedDisplay = ref(1);
const data = ref(defaultData);
const selectedWorkout = ref(-1);

/*

Özetle, bu computed property, seçili antrenmandaki tüm egzersizlerin giriş değerlerinin dolu olup olmadığını
kontrol eder ve tüm veriler doluysa true, aksi halde false döndürür.
*/
const isWorkoutComplete = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value];
  console.log("currWorkout", currWorkout);
  console.log("dataValue", data.value);
  console.log("seşec", selectedWorkout);
  if (!currWorkout) {
    return false;
  } // guard clause to exit function

  const isCompleteCheck = Object.values(currWorkout).every((ex) => !!ex);
  console.log("ISCOMPLETE: ", isCompleteCheck);
  return isCompleteCheck;
});

const firstInCompleteWorkoutIndex = computed(() => {
  const allWorkouts = data.value;
  if (!allWorkouts) return -1;

  // loop over every key value pair and check if the workout is complete or not
  for (const [index, workout] of Object.entries(allWorkouts)) {
    const isComplete = Object.values(workout).every((ex) => !!ex);
    if (!isComplete) {
      return parseInt(index);
    }
  }
  return -1; // all are complete
});

function handleChangeDisplay(idx) {
  selectedDisplay.value = idx;
}

function handleSelectWorkout(idx) {
  selectedDisplay.value = 3;
  selectedWorkout.value = idx;
}

function handleSaveWorkout() {
  // save the current data snapshot to localstorage so that we can retrieve it next time
  localStorage.setItem("workouts", JSON.stringify(data.value));

  // show the dashboard
  selectedDisplay.value = 2;

  // deselect workout
  selectedWorkout.value = -1;
}

function handleResetPlan() {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage.removeItem("workouts");
}

onMounted(() => {
  console.log("Page has mounded");

  if (!localStorage) {
    return;
  }

  if (localStorage.getItem("workouts")) {
    // only enter the if block if we find some data saved to  the key workouts in localstorage database
    const savedData = JSON.parse(localStorage.getItem("workouts"));
    data.value = savedData;
    selectedDisplay.value = 2; // if they have data, then we don't want them landing on the welcome screen ever time they enter the app
  }
});
</script>

<template>
  <Layout>
    <!-- PAGE 1 -->
    <Welcome
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay == 1"
    />
    <!-- PAGE 2 -->
    <Dashboard
      :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex"
      :handleSelectWorkout="handleSelectWorkout"
      :handleResetPlan="handleResetPlan"
      v-if="selectedDisplay == 2"
    />
    <!-- PAGE 3 -->
    <Workout
      :handleSaveWorkout="handleSaveWorkout"
      :isWorkoutComplete="isWorkoutComplete"
      :data="data"
      :selectedWorkout="selectedWorkout"
      v-if="workoutProgram?.[selectedWorkout]"
    />
  </Layout>
</template>

<style scoped></style>
