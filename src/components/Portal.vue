<template>
  <Teleport to="#portal">
    <div class="portal-container">
      <div role="button" class="portal-underlay" @click="props.handleCloseModal">
        <div class="portal-content">
          <slot />
        </div>
      </div>
    </div>
  </Teleport>
</template>

<script setup>
const props = defineProps({
  hello: String,
  handleCloseModal:Function
});
console.log("hello ", props.hello)
</script>

<style scoped>
.portal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 1001;
}

.portal-underlay {
  position: absolute;
  inset: 0;
  background: var(--background-primary);
  opacity: 0.7;
  z-index: 999;
  border: none;
  padding: none;
}

.portal-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 600px;
  max-width: 90vw;
  max-height: 80vh;
  display: grid;
  place-items: center;
  background: var(--border-secondary);
  border-radius: var(--border-radius-small);
  padding: 1rem;
  border: 1px solid var(--background-muted);
}
</style>
