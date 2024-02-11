<script setup>
import carsData from "../data.json"
import {ref, watch, onMounted} from "vue"
import {useRouter, useRoute} from "vue-router"
const router = useRouter()
const route = useRoute()
const cars = ref(carsData)
const make = ref("")
const price = ref("")
onMounted(() => {
  make.value = route.query.make || ""
  price.value = route.query.price || ""
})

watch([make, price], () => {
  let filteredCars = carsData;
  if (make.value && make.value !== "all") {
    filteredCars = filteredCars.filter(c => c.make === make.value);
  }
  if (price.value && price.value !== "all") {
    switch (price.value) {
      case "20000":
        filteredCars = filteredCars.filter(c => c.price <= 20000);
        break;
      case "50000":
        filteredCars = filteredCars.filter(c => c.price > 20000 && c.price <= 50000);
        break;
      case "300000":
        filteredCars = filteredCars.filter(c => c.price > 50000 && c.price <= 300000);
        break;
      default:
        filteredCars= null;
        break;
    }
  }
  cars.value = filteredCars;
});

function handleChange() {
  router.push({query: {
    make: make.value,
    price: price.value
  }})
}
</script>

<template>
  <main class="container">
    <h1>Our Cars</h1>
    <select @change="handleChange" v-model="make">
      <option value="all">All</option>
      <option value="Chevrolet">Chevrolet</option>
      <option value="Porsche">Porsche</option>
      <option value="Audi">Audi</option>
    </select>
    <select @change="handleChange" v-model="price">
      <option value="all">All</option>
      <option value="20000">max - $20.000</option>
      <option value="50000">$20.000 - $50.000</option>
      <option value="300000">$50.000 - $300.000</option>
    </select>
    <div class="cards">
      <template v-if="cars.length">
        <div 
          v-for="car in cars" 
          :key="car.id" 
          class="card"
          @click="router.push(`/car/${car.id}`)"
        >
          <h1>{{ car.make }}</h1>
          <p v-if="price && make">${{ car.price }}</p>
        </div>
      </template>
      <p v-else>No cars found with the selected filters</p>
    </div>
  </main>
</template>

<style scoped>
  .cards {
  display: flex;
  width: 1000px;
  flex-wrap: wrap;
  margin-top: 50px;
  justify-content: center;
}
.card {
  box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.207);
  padding: 15px;
  width: 150px;
  margin-right: 15px;
  cursor: pointer;
  margin-bottom: 20px;
}
</style>
