<template>
  <div>
    <input type="checkbox" :checked="done" @change="$emit('toggle')" />
    <!--<span :class="{ 'task-done': done }">{{ task }}</span> -->
    <span v-if="done" style="text-decoration: line-through">{{ task }}</span>
    <span v-else>{{ task }}</span>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  task: String,
  done: Boolean,
});
</script>

<style scoped>
div {
  display: flex;
  align-items: center;
  min-height: 3rem;
  gap: 1rem;
  border-radius: 5px;
  border: 1px solid #3f3f46;
}

input[type="checkbox"] {
  width: 15px;
  height: 15px;
  cursor: pointer;
}

label {
  font-size: 1.2rem;
}
</style>
