<script setup lang="ts">
import { ref, reactive, onMounted, onUnmounted, watch } from "vue";

const dots = reactive([]);
const returnAllDots = reactive([]);
const checker = reactive([]);
const returnPreviousDots = reactive([]);

let nextId = ref(0);

const pozitions = reactive({
  x: 0,
  y: 0,
});
console.log(pozitions);
const getPositions = (e) => {
  let horizont = e.pageX;
  let paralel = e.pageY;

  pozitions.x = horizont;
  pozitions.y = paralel;
};

const addDot = () => {
  const dot = {
    id: nextId.value++,
    horizontal: pozitions.x,
    paralel: pozitions.y,
  };

  dots.push(dot);
  returnAllDots.push(dot);
  checker.push(dot);
};

// const getLastDot = () => {
//   //   let previousDot = dott;
//   //   const lastDot = dots[dots.length - 1];

//   returnLastDot();
// };
const returnLastDot = () => {
  if (returnPreviousDots && returnPreviousDots.length > 0) {
    dots.push(returnPreviousDots.pop());
  } else {
    throw new Error("No dots");
  }
};

const returnDots = () => {
  if (returnAllDots.length > dots.length) {
    dots.push(...returnAllDots);
  } else {
    console.log("to much dots");
  }
};
const removeDot = () => {
  let removedDot = dots.pop();
  nextId.value--;
  if (dots.length === 0) {
    nextId.value = 0;
  }

  returnPreviousDots.push(removedDot);
};

onMounted(() => {
  window.addEventListener("click", getPositions);
  window.addEventListener("click", addDot);
});
onUnmounted(() => {
  window.addEventListener("click", getPositions);
  window.addEventListener("click", addDot);
});

watch(dots, () => {
  console.log(dots);
  console.log(returnAllDots);
});
// watch(returnPreviousDots, () => {
//   returnLastDot();
// });
</script>

<template>
  <div class="relative w-full h-[100vh] bg-black">
    <div
      v-if="dots"
      class="absolute"
      v-for="dot in dots"
      :key="dot"
      :style="{ left: dot.horizontal + 'px', top: dot.paralel + 'px' }"
    >
      <div class="text-white rounded-full w-4 h-4 bg-white text-[1px]">
        {{ dot.id }}
      </div>
    </div>
    <div class="flex gap-4">
      <button class="text-black bg-white m-8" @click.stop="removeDot">
        Remove
      </button>

      <button class="text-black bg-white m-8" @click.stop="returnDots">
        Return all dots
      </button>

      <button class="text-black bg-white m-8" @click.stop="returnLastDot">
        Return previous dots
      </button>
    </div>
  </div>
</template>
