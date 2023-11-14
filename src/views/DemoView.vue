<script setup lang="ts">
import { ref } from 'vue'
import IconBlub from '@/components/icons/IconBlub.vue'

const isLedOpen = ref(false)
const currentLedStatus = ref(false)
// const currentLedStatus = ref(true)

const switchStatus = () => {
  isLedOpen.value = !isLedOpen.value
}

const handleSubmitLedStatus = () => {
  console.log(isLedOpen.value)
  fetch('http://localhost:3000/api/hardware/led', {
    method: 'POST',
    body: JSON.stringify({ data: isLedOpen.value }),
    headers: {
      'Content-Type': 'application/json'
    }
  })
    .then((res) => res.json())
    .then((res) => {
      console.log(res)
      currentLedStatus.value = isLedOpen.value
    })
}
</script>

<template>
  <div>
    <div>
      <IconBlub :class="currentLedStatus ? 'light' : ''" />
    </div>
    <p>current: {{ currentLedStatus ? 'open' : 'close' }}</p>
    <p>target: {{ isLedOpen ? 'open' : 'close' }}</p>
    <div>
      <button @click="switchStatus">调节</button>
      <button @click="handleSubmitLedStatus">发送</button>
    </div>
  </div>
</template>

<style>
.light {
  fill: yellow;
}
</style>
