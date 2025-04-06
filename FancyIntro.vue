<template>

  <div style="margin: auto;
    position: relative;
    top: 190px;">
    <button
      class="p-3 hover:bg-white/20 rounded-full disabled:opacity-50 transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-white/50"
      style="    border: 1px solid #5992f6; position: relative;
    z-index: 11111;
    cursor: pointer;
    top: -160px;
    left: 20px;">

      <router-link to="/Explaination"> <span class="text-3xl font-light select-none">→</span></router-link>
    </button>
    <div class="outer" ref="outerRef" @click="handleStages">
      <div class="inner inner--1">
        <div class="orb orb--1"></div>
        <div class="orb orb--2"></div>
        <div class="orb orb--3"></div>
        <div class="orb orb--4"></div>
        <div class="orb orb--5"></div>
      </div>
      <div class="inner inner--2">
        <div class="orb orb--1"></div>
        <div class="orb orb--2"></div>
        <div class="orb orb--3"></div>
        <div class="orb orb--4"></div>
        <div class="orb orb--5"></div>
      </div>
      <h1 class="text text--1" >
        <strong class="slice slice--1">sit</strong>
        <strong class="slice slice--2">Relax </strong>
        <span class="slice slice--3">Enjoy</span>
      </h1>
      <h2 class="text text--2">
        <span class="slice slice--4">Optimisation</span>
        <strong class="slice slice--5">et efficacité</strong>
        <span class="slice slice--6">pour votre <strong>logistique !</strong></span>
      </h2>

      <p class="text text--3">
        <span class="slice slice--7">Optimisez votre <strong>logistique,</strong></span>
        <span class="slice slice--8"><strong>Routes, Dépôts</strong> et <strong>Flotte</strong></span>
        <span class="slice slice--9">grâce à une gestion</span>
        <span class="slice slice--10">intelligente et <strong>optimisée</strong></span>
      </p>


    </div>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <defs>
        <filter id="blend">
          <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
          <feColorMatrix
              in="blur"
              mode="matrix"
              values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7"
              result="blend"
          />
          <feBlend in="SourceGraphic" in2="blend" />
        </filter>
      </defs>
    </svg>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const stage = ref(0)
const outerRef = ref(null)

const handleStages = () => {
  if (stage.value === 0) {
    outerRef.value.classList.add('stage-1')
    stage.value++
    setTimeout(() => {
      outerRef.value.classList.add('stage-2')
      stage.value++
    }, 900)
  }
  if (stage.value === 2) {
    outerRef.value.classList.add('stage-3')
    stage.value++
    setTimeout(() => {
      outerRef.value.classList.add('stage-4')
      stage.value++
    }, 900)
  }
  if (stage.value === 4) {
    outerRef.value.classList.remove('stage-1', 'stage-2', 'stage-3', 'stage-4')
    stage.value = 0
  }
}

// Optional mouse movement handling
const handleMouseMove = (e) => {
  const cX = e.clientX
  const cY = e.clientY
  const degX = (cX * 0.02) - 12
  const degY = (cY * 0.02) - 12

  if (outerRef.value) {
    outerRef.value.style.transform = `rotateY(${degX}deg) rotateX(${degY}deg)`
  }
}

// Uncomment these to enable mouse movement effects

onMounted(() => {
  document.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  document.removeEventListener('mousemove', handleMouseMove)
})
</script>


<style scoped lang="less">
button {

  position: relative;
  z-index: 11111;
  cursor: pointer;

}

@dblue: #5992f6;
@lblue: #c2ede6;
@bg: #dcdfdf;
@white: #fff;

@easing: cubic-bezier(0.860, 0.000, 0.070, 1.000);

body {
  background-color: @bg;
  overflow: hidden;
  padding-top: 50px;
  cursor: pointer;
}



.outer {
  width: 865px;
  height: 605px;
  margin: auto;
  position: relative;
  transform-style: preserve-3d;

  &.d3.stage-1 {
    transform: rotateY(-10deg) rotateX(-10deg);
    transition: .9s ease;
  }

  &.d3.stage-2 {
    transform: rotateY(20deg) rotateX(20deg);
    transition: 2.3s ease;
  }

  &.d3.stage-3 {
    transform: rotateY(-10deg) rotateX(-10deg);
    transition: .8s ease;
  }

  &.d3.stage-4 {
    transform: rotateY(20deg) rotateX(20deg);
    transition: 2.3s ease;
  }
}

.inner {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
}

.inner--1 {
  display: none;

  .blend & {
    display: block;
    filter: url(#blend);
  }
}

.inner--2 {
  z-index: 2;
}



.text {
  font-size: 100px;
  line-height: .78;
  font-family: sans-serif;
  font-weight: bold;
  color: @white;
  text-transform: uppercase;
  -webkit-text-fill-color: transparent;
  -webkit-text-stroke-width: 2px;
  -webkit-text-stroke-color: @white;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
  z-index: 3;
  font-family: raleway;
  font-style: italic;
  white-space: nowrap;

  .d3 & {
    transform: translate3d(-50%, -50%, 80px);
  }

  strong {
    -webkit-text-fill-color: @white;
  }

  p& {
    -webkit-text-stroke-width: 1px;
    font-size: 60px;
    line-height: 1;
  }
}


.slice {
  display: block;
  transition: .8s @easing;

  .stage-1 & {
    opacity: 0;
  }
}

.slice--1 {
  .stage-1 & {
    transform: translateX(-100%);
  }
}
.slice--2 {
  transition-delay: .4s;

  .stage-1 & {
    transform: translateX(100%);
  }
}
.slice--3 {
  transition-delay: .7s;

  .stage-1 & {
    transform: translateX(-100%);
  }
}

.slice--4 {
  opacity: 0;
  transform: translateX(100%);

  .stage-2 & {
    opacity: 1;
    transform: translateX(0%);
  }

  .stage-3 & {
    opacity: 0;
    transform: translateX(100%);
  }
}
.slice--5 {
  opacity: 0;
  transform: translateX(-100%);
  transition-delay: .4s;

  .stage-2 & {
    opacity: 1;
    transform: translateX(0%);
  }

  .stage-3 & {
    opacity: 0;
    transform: translateX(-100%);
  }
}
.slice--6 {
  opacity: 0;
  transform: translateX(100%);
  transition-delay: .7s;

  .stage-2 & {
    opacity: 1;
    transform: translateX(0%);
  }

  .stage-3 & {
    opacity: 0;
    transform: translateX(100%);
  }
}
.slice--7 {
  opacity: 0;
  transform: translateY(100%);
  transition-delay: .4s;

  .stage-4 & {
    opacity: 1;
    transform: translateY(0%);
  }
}
.slice--8 {
  opacity: 0;
  transform: translateY(100%);
  transition-delay: .7s;

  .stage-4 & {
    opacity: 1;
    transform: translateY(0%);
  }
}
.slice--9 {
  opacity: 0;
  transform: translateY(100%);
  transition-delay: 1s;

  .stage-4 & {
    opacity: 1;
    transform: translateY(0%);
  }
}
.slice--10 {
  opacity: 0;
  transform: translateY(100%);
  transition-delay: 1.3s;

  .stage-4 & {
    opacity: 1;
    transform: translateY(0%);
  }
}



.orb {
  border-radius: 50%;
  position: absolute;
  transition: 1.4s @easing;

  &:before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    filter: blur(18px);
    border-radius: 50%;
    box-shadow: 0 0 8px @lblue;
    transition: 1.4s @easing;
    animation: move ease 10s infinite;

    .blend & {
      filter: blur(30px);
      box-shadow: 0 0 200px @dblue;
    }
  }
}

.orb--1 {
  width: 280px;
  height: 280px;
  left: 0;
  bottom: 50px;
  /* outline: 1px solid red; */

  &:before {
    background-image: radial-gradient(circle at bottom 35% right 32%, @dblue 35%, @lblue 75%);
  }

  .stage-2 & {
    transform: translate(0%, -20%) scale(4) rotate(100deg);
  }

  .stage-4 & {
    transform: translate(110%, 150%) scale(6) rotate(10deg);

    &:before {
      filter: blur(8px);
    }
  }
}


.orb--2 {
  width: 345px;
  height: 345px;
  left: 110px;
  top: 25px;
  /* outline: 1px solid orange; */

  &:before {
    background-image: radial-gradient(circle at bottom 0% left 48%, @dblue 35%, @lblue 75%);
    animation-delay: -18s;
  }

  .stage-2 & {
    transform: translate(-100%, -40%) rotate(30deg);

    &:before {
      filter: blur(8px);
    }
  }

  .stage-4 & {
    transform: translate(-40%, -80%) scale(3) rotate(40deg);

    &:before {
      filter: blur(4px);
    }
  }
}
.orb--3 {
  width: 340px;
  height: 340px;
  right: 290px;
  bottom: 0;
  /* outline: 1px solid pink; */

  &:before {
    background-image: radial-gradient(circle at bottom 40% left 12%, @dblue 25%, @lblue 85%);
    animation-delay: -14s;
  }

  .stage-2 & {
    transform: translate(150%, -120%) scale(2) rotate(-120deg);
  }

  .stage-4 & {
    transform: translate(100%, -50%) scale(1) rotate(-90deg);
  }
}
.orb--4 {
  width: 335px;
  height: 335px;
  right: 130px;
  top: 0;
  /* outline: 1px solid green; */

  &:before {
    background-image: radial-gradient(circle at bottom 0% left 42%, @dblue 35%, @lblue 75%);
    animation-delay: -4s;
  }

  .stage-2 & {
    transform: translate(-50%, 115%) rotate(-60deg);
    z-index: 4;

    &:before {
      filter: blur(8px);
    }
  }

  .stage-4 & {
    transform: translate(140%, -80%) scale(2) rotate(-90deg);

    &:before {
      filter: blur(12px);
    }
  }
}
.orb--5 {
  width: 290px;
  height: 290px;
  right: 0;
  bottom: 40px;

  &:before {
    background-image: radial-gradient(circle at bottom 0% left 35%, @dblue 20%, @lblue 60%);
    animation-delay: -24s;
  }

  .stage-2 & {
    transform: translate(50%, 80%) scale(1.5) rotate(-90deg);

    &:before {
      filter: blur(8px);
    }
  }

  .stage-4 & {
    transform: translate(-120%, 80%) scale(2) rotate(-100deg);
    z-index: 1;

    &:before {
      filter: blur(10px);
    }
  }
}

@keyframes move {
  0%{
    transform: scale(1) translate(0, 0);
  }
  33%{
    transform: scale(.9) translate(20px, 0);
  }
  68%{
    transform: scale(.8) translate(20px, 20px);
  }
  100%{
    transform: scale(1) translate(0, 0);
  }
}

/* Add all other styles here as per your original code. */
</style>
