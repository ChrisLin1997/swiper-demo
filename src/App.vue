<template>
<main>
  <div @click="handleArrow('left')" class="left-arrow"></div>
  <section ref="swiper" class="swiper">
    <article
      v-for="num of amout"
      class="swiper-item"
    >
      <img :src="`https://picsum.photos/${width}/400?random=${num}`" />
    </article>
  </section>
  <div @click="handleArrow('right')" class="right-arrow"></div>

  <footer>
    <div
      v-for="num of amout"
      :class="{ 'active' : activeImg === (num - 1) }"
      @click="handleDotClick(num - 1)"
    ></div>
  </footer>
</main>
</template>

<script setup>
import { computed, onMounted, ref, watchEffect } from 'vue'

const amout = 5
const width = 600
const swiper = ref(null)
const activeScrollPosition = ref(0)
const activeImg = computed(() => activeScrollPosition.value / width)
watchEffect(() => console.log(activeImg.value))

watchEffect(() => {
  console.log(activeScrollPosition.value)
  if (0 > activeScrollPosition.value) activeScrollPosition.value = (amout - 1) * width
  if (activeScrollPosition.value > (amout - 1) * width) activeScrollPosition.value = 0
})

onMounted(() => {
  watchEffect(() => swiper.value.scrollTo({ left: activeScrollPosition.value, behavior: 'smooth' }))
})

const handleArrow = (type) => {
  const mapping = {
    left: () => activeScrollPosition.value -= width,
    right: () => activeScrollPosition.value += width,
  }

  mapping[type]()
}

const handleDotClick = (value) => {
  activeScrollPosition.value = value * width
}

</script>

<style lang="scss" scoped>
main {
  margin: auto;
  position: relative;
  width: 600px;
  height: 400px;

  .left-arrow {
    position: absolute;
    width: 0px;
    height: 0px;
    left: 10px;
    top: calc(50% - 10px);
    border-right: 30px solid #00000099;
    border-top: 30px solid transparent;
    border-bottom: 30px solid transparent;
    cursor: pointer;
  }
  .right-arrow {
    position: absolute;
    width: 0px;
    height: 0px;
    right: 10px;
    top: calc(50% - 10px);
    border-left: 30px solid #00000099;
    border-top: 30px solid transparent;
    border-bottom: 30px solid transparent;
    cursor: pointer;
  }
}

section {
  display: flex;
  width: 600px;
  height: 400px;
  overflow: auto;
}

footer {
  position: absolute;
  bottom: 12px;
  display: flex;
  justify-content: center;
  width: 100%;

  * {
    margin: 0 16px;
    width: 10px;
    height: 10px;
    border-radius: 1000px;
    background: #fff;
    cursor: pointer;

    &:hover {
      opacity: .5;
    }
  }
}

.active {
  background: aqua;
}
</style>
