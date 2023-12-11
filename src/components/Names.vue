<template>
  <article class="flex flex-col items-center">
    <header class="text-2xl mb-10">
      {{ state.index + 1 }}/{{ listLength }}
    </header>
    <transition name="fade" mode="out-in">
      <h1 :key="state.index" style="font-size: 10rem; line-height: 0.6">
        {{ currentPerson.name }}
      </h1>
    </transition>

    <transition name="fade" mode="out-in">
      <div :key="state.index" class="flex justify-between w-full">
        <h3 class="text-5xl text-pink-500 mr-8">{{ currentPerson.stack }}</h3>
        <h2 class="text-5xl">
          <span v-if="state.index !== listLength - 1" class="text-pink-500"
            >Következő:
          </span>
          {{ nextPerson.name }}
        </h2>
      </div>
    </transition>
    <a
      href="#"
      class="mt-10 px-5 py-2 bg-pink-500 hover:bg-pink-600 text-white text-2xl rounded-lg inline-block shadow"
      @click.prevent="stepNext"
      >Következő</a
    >
  </article>
  <aside class="absolute top-0 right-0 h-full p-10 flex items-center">
    <ul class="text-right">
      <li
        v-for="person in people"
        :key="person.name"
        :class="[
          'text-2xl',
          person.name === currentPerson.name ? 'text-pink-500' : '',
        ]"
      >
        {{ person.name }}
      </li>
    </ul>
  </aside>
</template>

<script>
import { computed, reactive } from "vue";
import { useWindowEvent } from "../hooks/useWindowEvent";
import { NAMES } from "../constants";

export default {
  setup() {
    const state = reactive({
      index: 0,
    });

    const listLength = computed(() => NAMES.length);

    const currentPerson = computed(() => NAMES[state.index]);
    const nextPerson = computed(() => {
      const person = NAMES[state.index + 1];
      if (person) return person;
      return { name: "Nincs több név a listán" };
    });

    const stepNext = () => {
      const next = state.index++;
      if (next >= listLength.value - 1) state.index = 0;
    };

    const stepBack = () => {
      const previous = state.index--;
      if (previous <= 0) state.index = listLength.value - 1;
    };

    useWindowEvent("keydown", (event) => {
      console.log(event.key);
      switch (event.key) {
        case "ArrowDown":
        case "ArrowRight":
        case " ":
          event.preventDefault();
          stepNext();
          break;
        case "Backspace":
        case "ArrowUp":
        case "ArrowLeft":
          event.preventDefault();
          stepBack();
          break;
        default:
      }
    });

    return {
      currentPerson,
      nextPerson,
      stepNext,
      state,
      listLength,
      people: NAMES,
    };
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>