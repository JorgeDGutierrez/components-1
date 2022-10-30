<script setup>
defineProps({
  inicio: Number,
  fin: Number,
  maxlength: Number,
});
const emit = defineEmits(["next", "preview"]);
</script>

<template>
  <div class="btn-group" role="group" aria-label="Basic example">
    <button
      @click="emit('preview')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="inicio <= 0"
    >
      Previus {{ inicio }}
    </button>
    <button
      @click="emit('next')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="fin >= maxlength"
    >
      Next {{ fin }}
    </button>
  </div>
</template>
