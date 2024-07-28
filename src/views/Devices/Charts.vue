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

onBeforeMount(() => {
  socket.connect();

  if (undefined !== localStorage.theme && 'darkMode' === localStorage.theme){
    am4core.useTheme(am4themes_dark);
  } else {
    am4core.useTheme(am4themes_animated);
  }
});

onMounted(() => {
  initZoommableChart();
  initCustomChart();

  socket.emit('prueba');
});

const socket = io(`http://localhost:${import.meta.env.VITE_WEBSOCKET_PORT}`, {
  autoConnect: false
});

const zoommableChart = ref(null);
const initZoommableChart = () => {
  
  // Create chart instance
  let chart = am4core.create(zoommableChart.value, am4charts.XYChart);

  // Add data
  chart.data = [{
    "date": "2023-07-27",
    "value": 13
  }, {
    "date": "2023-07-28",
    "value": 11
  }, {
    "date": "2023-07-29",
    "value": 15
  }, {
    "date": "2023-07-30",
    "value": 16
  }, {
    "date": "2023-07-31",
    "value": 18
  }, {
    "date": "2023-08-01",
    "value": 13
  }, {
    "date": "2023-08-02",
    "value": 22
  }, {
    "date": "2023-08-03",
    "value": 23
  }, {
    "date": "2023-08-04",
    "value": 20
  }, {
    "date": "2023-08-05",
    "value": 17
  }, {
    "date": "2023-08-06",
    "value": 16
  }, {
    "date": "2023-08-07",
    "value": 18
  }, {
    "date": "2023-08-08",
    "value": 21
  }, {
    "date": "2023-08-09",
    "value": 26
  }, {
    "date": "2023-08-10",
    "value": 24
  }, {
    "date": "2023-08-11",
    "value": 29
  }, {
    "date": "2023-08-12",
    "value": 32
  }, {
    "date": "2023-08-13",
    "value": 18
  }, {
    "date": "2023-08-14",
    "value": 24
  }, {
    "date": "2023-08-15",
    "value": 22
  }, {
    "date": "2023-08-16",
    "value": 18
  }, {
    "date": "2023-08-17",
    "value": 19
  }, {
    "date": "2023-08-18",
    "value": 14
  }, {
    "date": "2023-08-19",
    "value": 15
  }, {
    "date": "2023-08-20",
    "value": 12
  }, {
    "date": "2023-08-21",
    "value": 8
  }, {
    "date": "2023-08-22",
    "value": 9
  }, {
    "date": "2023-08-23",
    "value": 8
  }, {
    "date": "2023-08-24",
    "value": 7
  }, {
    "date": "2023-08-25",
    "value": 5
  }, {
    "date": "2023-08-26",
    "value": 11
  }, {
    "date": "2023-08-27",
    "value": 13
  }, {
    "date": "2023-08-28",
    "value": 18
  }, {
    "date": "2023-08-29",
    "value": 20
  }, {
    "date": "2023-08-30",
    "value": 29
  }, {
    "date": "2023-08-31",
    "value": 33
  }, {
    "date": "2023-09-01",
    "value": 42
  }, {
    "date": "2023-09-02",
    "value": 35
  }, {
    "date": "2023-09-03",
    "value": 31
  }, {
    "date": "2023-09-04",
    "value": 47
  }, {
    "date": "2023-09-05",
    "value": 52
  }, {
    "date": "2023-09-06",
    "value": 46
  }, {
    "date": "2023-09-07",
    "value": 41
  }, {
    "date": "2023-09-08",
    "value": 43
  }, {
    "date": "2023-09-09",
    "value": 40
  }, {
    "date": "2023-09-10",
    "value": 39
  }, {
    "date": "2023-09-11",
    "value": 34
  }, {
    "date": "2023-09-12",
    "value": 29
  }, {
    "date": "2023-09-13",
    "value": 34
  }, {
    "date": "2023-09-14",
    "value": 37
  }, {
    "date": "2023-09-15",
    "value": 42
  }, {
    "date": "2023-09-16",
    "value": 49
  }, {
    "date": "2023-09-17",
    "value": 46
  }, {
    "date": "2023-09-18",
    "value": 47
  }, {
    "date": "2023-09-19",
    "value": 55
  }, {
    "date": "2023-09-20",
    "value": 59
  }, {
    "date": "2023-09-21",
    "value": 58
  }, {
    "date": "2023-09-22",
    "value": 57
  }, {
    "date": "2023-09-23",
    "value": 61
  }, {
    "date": "2023-09-24",
    "value": 59
  }, {
    "date": "2023-09-25",
    "value": 67
  }, {
    "date": "2023-09-26",
    "value": 65
  }, {
    "date": "2023-09-27",
    "value": 61
  }, {
    "date": "2023-09-28",
    "value": 66
  }, {
    "date": "2023-09-29",
    "value": 69
  }, {
    "date": "2023-09-30",
    "value": 71
  }, {
    "date": "2023-10-01",
    "value": 67
  }, {
    "date": "2023-10-02",
    "value": 63
  }, {
    "date": "2023-10-03",
    "value": 46
  }, {
    "date": "2023-10-04",
    "value": 32
  }, {
    "date": "2023-10-05",
    "value": 21
  }, {
    "date": "2023-10-06",
    "value": 18
  }, {
    "date": "2023-10-07",
    "value": 21
  }, {
    "date": "2023-10-08",
    "value": 28
  }, {
    "date": "2023-10-09",
    "value": 27
  }, {
    "date": "2023-10-10",
    "value": 36
  }, {
    "date": "2023-10-11",
    "value": 33
  }, {
    "date": "2023-10-12",
    "value": 31
  }, {
    "date": "2023-10-13",
    "value": 30
  }, {
    "date": "2023-10-14",
    "value": 34
  }, {
    "date": "2023-10-15",
    "value": 38
  }, {
    "date": "2023-10-16",
    "value": 37
  }, {
    "date": "2023-10-17",
    "value": 44
  }, {
    "date": "2023-10-18",
    "value": 49
  }, {
    "date": "2023-10-19",
    "value": 53
  }, {
    "date": "2023-10-20",
    "value": 57
  }, {
    "date": "2023-10-21",
    "value": 60
  }, {
    "date": "2023-10-22",
    "value": 61
  }, {
    "date": "2023-10-23",
    "value": 69
  }, {
    "date": "2023-10-24",
    "value": 67
  }, {
    "date": "2023-10-25",
    "value": 72
  }, {
    "date": "2023-10-26",
    "value": 77
  }, {
    "date": "2023-10-27",
    "value": 75
  }, {
    "date": "2023-10-28",
    "value": 70
  }, {
    "date": "2023-10-29",
    "value": 72
  }, {
    "date": "2023-10-30",
    "value": 70
  }, {
    "date": "2023-10-31",
    "value": 72
  }, {
    "date": "2023-11-01",
    "value": 73
  }, {
    "date": "2023-11-02",
    "value": 67
  }, {
    "date": "2023-11-03",
    "value": 68
  }, {
    "date": "2023-11-04",
    "value": 65
  }, {
    "date": "2023-11-05",
    "value": 71
  }, {
    "date": "2023-11-06",
    "value": 75
  }, {
    "date": "2023-11-07",
    "value": 74
  }, {
    "date": "2023-11-08",
    "value": 71
  }, {
    "date": "2023-11-09",
    "value": 76
  }, {
    "date": "2023-11-10",
    "value": 77
  }, {
    "date": "2023-11-11",
    "value": 81
  }, {
    "date": "2023-11-12",
    "value": 83
  }, {
    "date": "2023-11-13",
    "value": 80
  }, {
    "date": "2023-11-14",
    "value": 81
  }, {
    "date": "2023-11-15",
    "value": 87
  }, {
    "date": "2023-11-16",
    "value": 82
  }, {
    "date": "2023-11-17",
    "value": 86
  }, {
    "date": "2023-11-18",
    "value": 80
  }, {
    "date": "2023-11-19",
    "value": 87
  }, {
    "date": "2023-11-20",
    "value": 83
  }, {
    "date": "2023-11-21",
    "value": 85
  }, {
    "date": "2023-11-22",
    "value": 84
  }, {
    "date": "2023-11-23",
    "value": 82
  }, {
    "date": "2023-11-24",
    "value": 73
  }, {
    "date": "2023-11-25",
    "value": 71
  }, {
    "date": "2023-11-26",
    "value": 75
  }, {
    "date": "2023-11-27",
    "value": 79
  }, {
    "date": "2023-11-28",
    "value": 70
  }, {
    "date": "2023-11-29",
    "value": 73
  }, {
    "date": "2023-11-30",
    "value": 61
  }, {
    "date": "2023-12-01",
    "value": 62
  }, {
    "date": "2023-12-02",
    "value": 66
  }, {
    "date": "2023-12-03",
    "value": 65
  }, {
    "date": "2023-12-04",
    "value": 73
  }, {
    "date": "2023-12-05",
    "value": 79
  }, {
    "date": "2023-12-06",
    "value": 78
  }, {
    "date": "2023-12-07",
    "value": 78
  }, {
    "date": "2023-12-08",
    "value": 78
  }, {
    "date": "2023-12-09",
    "value": 74
  }, {
    "date": "2023-12-10",
    "value": 73
  }, {
    "date": "2023-12-11",
    "value": 75
  }, {
    "date": "2023-12-12",
    "value": 70
  }, {
    "date": "2023-12-13",
    "value": 77
  }, {
    "date": "2023-12-14",
    "value": 67
  }, {
    "date": "2023-12-15",
    "value": 62
  }, {
    "date": "2023-12-16",
    "value": 64
  }, {
    "date": "2023-12-17",
    "value": 61
  }, {
    "date": "2023-12-18",
    "value": 59
  }, {
    "date": "2023-12-19",
    "value": 53
  }, {
    "date": "2023-12-20",
    "value": 54
  }, {
    "date": "2023-12-21",
    "value": 56
  }, {
    "date": "2023-12-22",
    "value": 59
  }, {
    "date": "2023-12-23",
    "value": 58
  }, {
    "date": "2023-12-24",
    "value": 55
  }, {
    "date": "2023-12-25",
    "value": 52
  }, {
    "date": "2023-12-26",
    "value": 54
  }, {
    "date": "2023-12-27",
    "value": 50
  }, {
    "date": "2023-12-28",
    "value": 50
  }, {
    "date": "2023-12-29",
    "value": 51
  }, {
    "date": "2023-12-30",
    "value": 52
  }, {
    "date": "2023-12-31",
    "value": 58
  }, {
    "date": "2024-01-01",
    "value": 60
  }, {
    "date": "2024-01-02",
    "value": 67
  }, {
    "date": "2024-01-03",
    "value": 64
  }, {
    "date": "2024-01-04",
    "value": 66
  }, {
    "date": "2024-01-05",
    "value": 60
  }, {
    "date": "2024-01-06",
    "value": 63
  }, {
    "date": "2024-01-07",
    "value": 61
  }, {
    "date": "2024-01-08",
    "value": 60
  }, {
    "date": "2024-01-09",
    "value": 65
  }, {
    "date": "2024-01-10",
    "value": 75
  }, {
    "date": "2024-01-11",
    "value": 77
  }, {
    "date": "2024-01-12",
    "value": 78
  }, {
    "date": "2024-01-13",
    "value": 70
  }, {
    "date": "2024-01-14",
    "value": 70
  }, {
    "date": "2024-01-15",
    "value": 73
  }, {
    "date": "2024-01-16",
    "value": 71
  }, {
    "date": "2024-01-17",
    "value": 74
  }, {
    "date": "2024-01-18",
    "value": 78
  }, {
    "date": "2024-01-19",
    "value": 85
  }, {
    "date": "2024-01-20",
    "value": 82
  }, {
    "date": "2024-01-21",
    "value": 83
  }, {
    "date": "2024-01-22",
    "value": 88
  }, {
    "date": "2024-01-23",
    "value": 85
  }, {
    "date": "2024-01-24",
    "value": 85
  }, {
    "date": "2024-01-25",
    "value": 80
  }, {
    "date": "2024-01-26",
    "value": 87
  }, {
    "date": "2024-01-27",
    "value": 84
  }, {
    "date": "2024-01-28",
    "value": 83
  }, {
    "date": "2024-01-29",
    "value": 84
  }, {
    "date": "2024-01-30",
    "value": 81
  }];

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

