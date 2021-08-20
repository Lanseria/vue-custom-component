<template>
  <button aria-label="Toggle theme" @click="toggleDark()">
    <transition name="slide" mode="out-in">
      <div v-if="isDark">暗色</div>
      <div v-else>浅色</div>
    </transition>
  </button>
</template>

<script lang="ts">
import { defineComponent, onMounted, watch } from "vue";
import { useDark, useToggle } from "@vueuse/core";
import { useShadowHost } from "./utils";
export default defineComponent({
  name: "DarkModeSwitchCe",
  setup() {
    const isDark = useDark();
    const toggleDark = useToggle(isDark);
    const host = useShadowHost();
    function reflectDark() {
      host.value?.toggleAttribute("dark", isDark.value);
    }
    onMounted(reflectDark);
    watch(isDark, reflectDark);
    return {
      isDark,
      toggleDark,
    };
  },
});
</script>

<style>
:host {
  --color: #fbbf24;
  --bg-normal: #fafaf9;
  --bg-active: #f5f5f4;
  --font-size: 24px;
}

:host([dark]) {
  --color: #fef3c7;
  --bg-normal: #262626;
  --bg-active: #2d2d2d;
}

button {
  background-color: var(--bg-normal);
  border: none;
  border-radius: 0.5rem;
  color: var(--color);
  cursor: pointer;
  display: flex;
  font-size: var(--font-size);
  overflow: hidden;
  padding: 0.4em;
  transition: background-color 0.3s ease,
    color 0.3s cubic-bezier(0.64, 0, 0.78, 0);
}

button:hover,
button:focus {
  background-color: var(--bg-active);
  outline: none;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease-out;
}

.slide-enter-from {
  transform: translateY(-150%);
}

.slide-enter-to,
.slide-leave-from {
  transform: translateY(0);
}

.slide-leave-to {
  transform: translateY(150%);
}
</style>
