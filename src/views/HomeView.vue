<template>
  <main class="main">
    <v-row>
      <v-col cols="12">
        <h1>تسک مصاحبه شرکت سامایش</h1>
        <div class="temp-row" v-for="(item, i) in tempData" :key="i">
          <span>نام سنسور : {{ item.sensorName }}</span>
          <span>دما : {{ item.temperature }}</span>
          <span>رطوبت : {{ item.humidity }}</span>
          <span>ولتاژ : {{ item.Voltage }}</span>
          <v-btn @click="showGraph(item.id)">مشاهده نمودار</v-btn>
        </div>
        <v-dialog v-model="graphDialog" width="800">
          <v-card max-width="800" title="نمودار 8 ساعت گذشته">
            <graphComponent :graphData="graphData" />
            <template v-slot:actions>
              <v-btn class="ms-auto" text="بستن" @click="graphDialog = false"></v-btn>
            </template>
          </v-card>
        </v-dialog>
      </v-col>
    </v-row>
  </main>
</template>

<script setup lang="ts">

import axios from 'axios';
import { onMounted, ref, watch, } from 'vue';
import graphComponent from '../components/graphComponent.vue'




const tempData = ref('');
const graphData = ref([]);
const graphDialog = ref(false);





const showGraph = (item: number) => {
  graphDialog.value = true;
  getItem(item)
}

const getList = () => {
  axios.get('https://66784d040bd45250561e3744.mockapi.io/tempreture').then(
    (response) => {
      tempData.value = response.data
    }
  )
}

const getItem = (id: number) => {
  graphData.value = []
  axios.get(`https://66784d040bd45250561e3744.mockapi.io/tempreture/${id}`).then(
    (response) => {
      let data = response.data.temp
      data.forEach((item: any) => {
        graphData.value.push(item.data)
      });
    }
  )
}

const generateRandomTemp = () => {
  return (Math.random() + 10).toFixed(2);
}

const updateVoltage = () => {
  tempData.value = tempData.value.map((sensor) => ({
    ...sensor,
    Voltage: generateRandomTemp()
  }));
}


onMounted(() => {
  getList()
  setInterval(updateVoltage,60000)
})


</script>

<style>
.main {
  margin: 2rem;
}

h1{
  margin: 2rem 0;
}

.description {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.temp-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #3b3b3b;
}
</style>