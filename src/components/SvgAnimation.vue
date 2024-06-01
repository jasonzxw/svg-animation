<template>
  <svg
    version="1.1"
    baseProfile="full"
    :width="svgInfo.width"
    :height="svgInfo.height"
    xmlns="http://www.w3.org/2000/svg"
    class="svg_container"
    ref="containerRef"
    @mousemove="leftMousemove"
  >
    <circle
      :cx="svgInfo.leftCircleStartpoint.x"
      :cy="svgInfo.leftCircleStartpoint.y"
      :r="svgInfo.r"
      fill="#792fff"
      ref="leftCircle"
      @mousedown="leftMousedown"
      @mouseup="leftMouseup"
      id="leftcircle"
    />
    <line
      :x1="svgInfo.leftRefStartInfo.x"
      :x2="svgInfo.leftCircleStartpoint.x"
      :y1="svgInfo.leftRefStartInfo.y"
      :y2="svgInfo.leftCircleStartpoint.y"
      stroke="#792fff"
      stroke-width="5"
    />

    <circle
      :cx="svgInfo.rightCircleStartpoint.x"
      :cy="svgInfo.rightCircleStartpoint.y"
      :r="svgInfo.r"
      fill="#792fff"
      ref="rightCircle"
      @mousedown="rightMousedown"
    />
    <line
      :x1="svgInfo.rightRefStartInfo.x"
      :x2="svgInfo.rightCircleStartpoint.x"
      :y1="svgInfo.rightRefStartInfo.y"
      :y2="svgInfo.rightCircleStartpoint.y"
      stroke="#792fff"
      stroke-width="5"
    />

    <line
      :x1="svgInfo.leftRefStartInfo.x"
      :x2="svgInfo.rightRefStartInfo.x"
      :y1="svgInfo.leftRefStartInfo.y"
      :y2="svgInfo.rightRefStartInfo.y"
      stroke="gray"
      stroke-width="5"
    />

    <path
      stroke="#868686"
      strokeOpacity="1" fill="none"
      pointerEvents="visibleStroke"
      fillOpacity="1"
      troke="#792fff"
      :d="becir"></path>
  </svg>
</template>

<script lang="ts" setup>
import { reactive, ref, shallowRef, onUnmounted, computed } from "vue";

const containerRef = shallowRef<SVGElement>();
const leftRef = shallowRef<SVGLineElement>();
const rightRef = shallowRef<SVGLineElement>();
const leftCircle = shallowRef<SVGCircleElement>();
const rightCircle = shallowRef<SVGCircleElement>();
const svgInfo = reactive({
  width: 300,
  height: 300,
  leftRefStartInfo: {
    x: 20,
    y: 200,
  },
  rightRefStartInfo: {
    x: 220,
    y: 30,
  },
  leftCircleStartpoint: {
    x: 60,
    y: 200,
  },
  rightCircleStartpoint: {
    x: 180,
    y: 30,
  },
  r: 5,
});

const center = ref({
    x: 100,
    y: 100
})

const becir = computed(() => {
    let x = `M ${svgInfo.leftRefStartInfo.x} ${svgInfo.leftRefStartInfo.y} Q ${center.value.x} ${center.value.y} ${svgInfo.rightRefStartInfo.x} ${svgInfo.rightRefStartInfo.y}`
    return x
})
const mouseinfo = ref({
  leftMove: false,
  rightMove: false,
});
const leftMousedown = () => {
  mouseinfo.value.leftMove = true;
};
const leftMousemove = (e: MouseEvent) => {
  if (mouseinfo.value.leftMove) {
    const container = containerRef.value.getBoundingClientRect();
    svgInfo.leftCircleStartpoint.x = center.value.x = e.clientX - container.left;
    svgInfo.leftCircleStartpoint.y = center.value.y =e.clientY - container.top;
  }
  if (mouseinfo.value.rightMove) {
    const container = containerRef.value.getBoundingClientRect();
    svgInfo.rightCircleStartpoint.x = center.value.x =  e.clientX - container.left;
    svgInfo.rightCircleStartpoint.y = center.value.y = e.clientY - container.top;
  }
};

const rightMousedown = () => {
  mouseinfo.value.rightMove = true;
};

const leftMouseup = (e: MouseEvent) => {
  mouseinfo.value.leftMove = false;
  mouseinfo.value.rightMove = false;
};

document.addEventListener("mouseup", leftMouseup);

onUnmounted(() => document.removeEventListener("mouseup", leftMouseup));
</script>

<style scoped>
.svg_container {
  border: 1px solid black;
}
</style>
