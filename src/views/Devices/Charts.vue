<template>
  <v-row>
    <!-- Columna 1 -->
    <v-col>
      <v-card title="Grafico 1" color="pink-darken-3" variant="tonal" height="400px">
        <div class="h-chart" ref="zoommableChart"></div>
      </v-card>
    </v-col>
    <!-- Columna 2 -->
    <v-col>
      <v-card title="Grafico 2" color="pink-darken-3" variant="tonal" height="400px">
        <div class="h-chart" ref="customChart"></div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script setup>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import am4themes_dark from "@amcharts/amcharts4/themes/dark";
import { onBeforeMount, onMounted, ref } from "vue";
import { io } from "socket.io-client";
import { useRoute } from "vue-router";

const route = useRoute();

onBeforeMount(() => {
  socket.connect();
  
  if (undefined !== localStorage.theme && 'darkMode' === localStorage.theme){
    am4core.useTheme(am4themes_dark);
  } else {
    am4core.useTheme(am4themes_animated);
  }
});

onMounted(() => {
  socket.emit('inicio', route.params.key);

  initZoommableChart();
  initCustomChart();

  socket.on('dispositivo', (dispositivo) => {
    console.log('Dispositivo no encontrado');    
  });
});

const socket = io(`http://localhost:${import.meta.env.VITE_WEBSOCKET_PORT}`, {
  autoConnect: false
});

const zoommableChart = ref(null);
const initZoommableChart = () => {
  
  // Create chart instance
  let chart = am4core.create(zoommableChart.value, am4charts.XYChart);

  // Add data
  //chart.data = [
  //  {"date": "2024-01-29","value": 84},
  //  {"date": "2024-01-30","value": 81}
  //];

  socket.on('temperatura', ({date, value}) => {
    console.log('Temperatura: ', value);
    
    chart.addData({date: new Date(date), value: value });
  });


  // Create axes
  let dateAxis = chart.xAxes.push(new am4charts.DateAxis());
  dateAxis.renderer.grid.template.location = 0;
  dateAxis.renderer.minGridDistance = 50;

  let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());

  // Create series
  let series = chart.series.push(new am4charts.LineSeries());
  series.dataFields.valueY = "value";
  series.dataFields.dateX = "date";
  series.strokeWidth = 3;
  series.fillOpacity = 0.5;

  // Add vertical scrollbar
  chart.scrollbarY = new am4core.Scrollbar();
  chart.scrollbarY.marginLeft = 0;

  // Add cursor
  chart.cursor = new am4charts.XYCursor();
  chart.cursor.behavior = "zoomY";
  chart.cursor.lineX.disabled = true;
}

const customChart = ref(null);
const initCustomChart = () => {
  
  // Create chart instance
  let chart = am4core.create(customChart.value, am4charts.XYChart3D);

  // Add data
  chart.data = [{
    "year": 2005,
    "income": 23.5,
    "color": chart.colors.next()
  }, {
    "year": 2006,
    "income": 26.2,
    "color": chart.colors.next()
  }, {
    "year": 2007,
    "income": 30.1,
    "color": chart.colors.next()
  }, {
    "year": 2008,
    "income": 29.5,
    "color": chart.colors.next()
  }, {
    "year": 2009,
    "income": 24.6,
    "color": chart.colors.next()
  }];

  // Create axes
  let categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
  categoryAxis.dataFields.category = "year";
  categoryAxis.numberFormatter.numberFormat = "#";
  categoryAxis.renderer.inversed = true;

  let valueAxis = chart.xAxes.push(new am4charts.ValueAxis());

  // Create series
  let series = chart.series.push(new am4charts.ColumnSeries3D());
  series.dataFields.valueX = "income";
  series.dataFields.categoryY = "year";
  series.name = "Income";
  series.columns.template.propertyFields.fill = "color";
  series.columns.template.tooltipText = "{valueX}";
  series.columns.template.column3D.stroke = am4core.color("#fff");
  series.columns.template.column3D.strokeOpacity = 0.2;
}
</script>

<style>
.h-chart {
  height: 350px;
}
</style>

