<script lang="ts">
import { ref, onMounted } from 'vue'
import ButtonSVG from '@/assets/img/ButtonSVG.svg'
import DivideSVG from '@/assets/img/DivideSVG.svg'
type Advice = {
  id: number
  advice: string
}

type Data = {
  slip: Advice
}
const fetcher = async <T = any,>(url: string): Promise<T> => {
  const request = await fetch(url)
  return request.json()
}

const getData = (): Promise<Data> => fetcher<Data>('https://api.adviceslip.com/advice')

export default {
  setup() {
    const slip = ref<Advice>({
      id: 0,
      advice: ''
    })
    console.log(slip)

    const refreshAdvice = async () => {
      slip.value = await getData().then((data) => data.slip)
    }

    onMounted(() => {
      refreshAdvice()
    })

    return { slip, refreshAdvice, ButtonSVG, DivideSVG }
  }
}
</script>

<template>
  <div class="flex relative w-full min-h-screen flex-col justify-center items-center bg-[#202733]">
    <div
      class="w-[33.75rem] h-[20.75rem] bg-[#313A48] rounded-[0.9375rem] flex flex-col items-center justify-center"
    >
      <h3 class="text-[#53ffaa] text-[0.8125rem] font-extrabold tracking-[0.25538rem]">
        ADVICE #{{ slip.id }}
      </h3>
      <p
        class="text-[#cee3e9] text-[1.75rem] w-[27.75rem] my-[1rem] text-center font-extrabold tracking-[-0.01875rem]"
      >
        {{ slip.advice }}
      </p>
      <div class="flex items-center justify-between">
        <div class="w-[12.25rem] h-[0.0625rem] bg-[#4F5D74]"></div>
        <div class="mx-[1rem]">
          <img :src="DivideSVG" alt="Divide SVG" />
        </div>
        <div className="w-[12.25rem] h-[0.0625rem] bg-[#4F5D74]"></div>
      </div>
    </div>
    <div
      class="mt-[-2rem] flex rounded-full hover:cursor-pointer bg-transparent border-none hover:shadow-[0px_0px_25px_#53ffaa]"
    >
      <button @click="refreshAdvice">
        <img :src="ButtonSVG" alt="Button SVG" />
      </button>
    </div>
  </div>
</template>
