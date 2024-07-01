<template>
  <div>
    <h1>Car Wheel Information</h1>
    <div>
      <label for="carName">Car name?</label>
      <input v-model="carName" id="carName" @keyup.enter="handleCarName"/>
      <button @click="handleCarName">Submit</button>
    </div>
    <div v-if="message">{{ message }}</div>
    <div v-if="showCarList">
      <h3>List of cars:</h3>
      <ul>
        <li v-for="car in carList" :key="car">{{ car }}</li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'CarWheelInfo',
  setup() {
    const carName = ref('');
    const message = ref('');
    const showCarList = ref(false);
    const carList = ref<string[]>([]);
    const carWheelMap = ref<{ [key: string]: number }>({});

    const handleCarName = () => {
      const name = carName.value.trim();
      if (name.toLowerCase() === 'bye') {
        message.value = 'Goodbye!';
        carName.value = '';
        return;
      }
      if (name.toLowerCase() === 'all') {
        showCarList.value = true;
        carList.value = Object.keys(carWheelMap.value);
        return;
      }
      if (name.toLowerCase() === '4-wheel-car') {
        showCarList.value = true;
        carList.value = Object.keys(carWheelMap.value).filter(car => carWheelMap.value[car] === 4);
        return;
      }
      if (name.toLowerCase() === '2-wheel-car') {
        showCarList.value = true;
        carList.value = Object.keys(carWheelMap.value).filter(car => carWheelMap.value[car] === 2);
        return;
      }
      showCarList.value = false;
      if (carWheelMap.value[name]) {
        message.value = `"${name}" has ${carWheelMap.value[name]} wheels.`;
      } else {
        const wheels = prompt(`"${name}" is not on my list. Number of wheels?`);
        if (wheels && !isNaN(Number(wheels))) {
          carWheelMap.value[name] = Number(wheels);
          message.value = `Thanks. I updated the information.`;
        } else {
          message.value = 'Invalid input. Please enter a valid number of wheels.';
        }
      }
      carName.value = '';
    };

    return {
      carName,
      message,
      showCarList,
      carList,
      handleCarName,
    };
  },
});
</script>

<style scoped>
h1 {
  font-size: 24px;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
}
input {
  margin: 0 10px;
}
button {
  margin-left: 10px;
}
</style>
