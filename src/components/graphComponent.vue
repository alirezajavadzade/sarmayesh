<template>
    <div>
        <v-chart class="chart" :option="option" />
    </div>
</template>

<script setup lang="ts">
import { ref, provide } from "vue";
import { GridComponent } from 'echarts/components';
import { LineChart } from 'echarts/charts';
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { BarChart } from 'echarts/charts';
import {
    TitleComponent,
    TooltipComponent,
    LegendComponent,
} from 'echarts/components';
import VChart, { THEME_KEY } from 'vue-echarts';


const props = defineProps(['graphData'])


use([
    CanvasRenderer,
    BarChart,
    TitleComponent,
    TooltipComponent,
    LegendComponent,
    GridComponent,
    LineChart
]);

provide(THEME_KEY, 'light');

const option = ref({
    xAxis: {
        type: 'category',
        data: ['1', '2', '3', '4', '5', '6', '7', '8']
    },
    yAxis: {
        type: 'value'
    },
    series: [
        {
            data: props.graphData,
            type: 'line',
            smooth: true
        }
    ]
});




</script>

<style scoped>
.chart {
    height: 80vh;
}
</style>