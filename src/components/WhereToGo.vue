<template>
  <BottomDialog>
    <div class="destinations pb-6 no-scrollbar overflow-y-scroll pt-8 flex flex-col justify-start items-center">
      <button
        class="destination rounded-3xl border-4 border-gray-300 p-3 mb-3 flex flex-row justify-start items-center space-x-4 mx-4"
        @click="newTrip()">
        <MapPinIcon class="icon bg-button-color rounded-xl p-1.5 box-content w-5 h-5" />
        <div class="flex-grow">
          <span class="flex flex-row space-x-4 text-xs">New Trip</span>
          <span class="description text-gray-500 font-medium">Tap for location</span>
        </div>
      </button>

      <Button
        class="destination rounded-3xl border-2 border-gray-300 p-3 mb-3 flex flex-row justify-start items-center space-x-4 mx-4"
        v-for="item in destinations" :key="item.id" @click="flyTo(item)">

        <component :is="item.icon" class="icon bg-button-color rounded-xl p-1.5 box-content w-5 h-5" />
        <div class="flex flex-row space-x-4 text-xs">
          <span class="font-bold ">{{ item.title }}</span>
          <span class="description font-medium">{{ item.description }}</span>
        </div>
      </Button>



    </div>

  </BottomDialog>
  
</template>

<script setup>
import { reactive } from 'vue'
import BottomDialog from '@/components/BottomDialog.vue'
import Button from '@/components/Button.vue'
import { MapPinIcon, HomeIcon, HomeModernIcon, BuildingStorefrontIcon, TrophyIcon } from '@heroicons/vue/24/outline'
import { useRouter } from 'vue-router'
import { map } from '@/store'

const router = useRouter()
const destinations = reactive([
  {
    title: 'Home',
    description: '15km, 20min',
    icon: HomeIcon,
    coordinates: [121.473, 31.2258245]
  },
  {
    title: 'Store',
    description: '18km, 23min',
    icon: BuildingStorefrontIcon,
    coordinates: [120.472553, 31.2228245]
  },
  {
    title: 'Work',
    description: '22km, 30min',
    icon: HomeModernIcon,
    coordinates: [121.417553, 31.2328245]
  },
  {
    title: 'Racing',
    description: '30km, 35min',
    icon: TrophyIcon,
    coordinates: [121.47553, 31.2628245]
  }
])

function newTrip() {
  router.push({
    name: 'pick-up'
  })
}

function flyTo(item) {
  map.value.flyTo({ center: item.coordinates, essential: true })
}
</script>
