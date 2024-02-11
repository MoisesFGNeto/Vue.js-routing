<script setup>
import {useRoute} from "vue-router"
import {ref, onBeforeMount} from "vue"
import cars from "../data.json"
const car = ref(null)
const route = useRoute()
const {id} = route.params

onBeforeMount(() => {
  car.value = cars.find(c => c.id === parseInt(id))
})
</script>

<template>
  <div class="container cards">
   <div v-if="car">
    <h1>The Car</h1>
    <p>Maker: {{ car.make }}</p>
    <p>Body: {{ car.body }}</p>
    <p>Price: {{ car.price }}</p>
    <p>Year: {{ car.year }}</p>
   </div>
   <div v-else>
    <h1>Car not found</h1>
   </div>
  </div>
</template>

<style scoped>
  .cards {
 border: 1px solid black;
 height: 300px;
 width: 250px;
 margin-top: 100px;
}
</style>