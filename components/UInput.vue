<template>
  <div class="flex flex-col">
    <label
      v-if="label"
      :for="id"
      class="mb-1 text-sm font-medium text-gray-700"
    >
      {{ label }}
    </label>
    <input
      :id="id"
      :type="type"
      :placeholder="placeholder"
      :min="min"
      :max="max"
      :value="modelValue"
      @input="onInput"
      class="px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:red"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  modelValue: string | number;
  label?: string;
  placeholder?: string;
  type?: "text" | "number" | "email" | "password";
  min?: number;
  max?: number;
  id?: string;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: string | number): void;
}>();

const id = computed(
  () => props.id || `input-${Math.random().toString(36).slice(2)}`,
);

function onInput(e: Event) {
  const input = e.target as HTMLInputElement;
  let value: string | number = input.value;

  if (props.type === "number") {
    value = input.valueAsNumber;
  }

  emit("update:modelValue", value);
}
</script>
