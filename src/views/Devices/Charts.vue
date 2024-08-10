<template>
  <v-row>
    <!-- Columna 1 -->
    <v-col cols="6">
      <v-card title="Gráfico 1" color="indigo-darken-3" variant="tonal" height="400px">
        <div class="h-chart" ref="zoomableChart"></div>
      </v-card>
    </v-col>

    <!-- Sección media -->
    <v-col cols="3">
      <v-row>
        <!-- Led -->
        <v-col cols="6">
          <v-card title="Led" color="indigo-darken-3" variant="tonal" height="150px">
            <v-card-text class="h-100 text-center">
              <!-- v-row>v-col>v-btn -> me genera las etiquetas de ambos elementos. -->
              <v-row class="h-100 mt-1">
                <v-col>
                  <v-btn
                    size="x-large"
                    :color="light ? 'yellow' : 'blue-grey'"
                    :icon="light ? 'mdi-lightbulb-on' : 'mdi-lightbulb-outline'"
                    @click="toggleLight()"
                  ></v-btn>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>

        <!-- Ventilador -->
        <v-col cols="6">
          <v-card title="Ventilador" color="indigo-darken-3" variant="tonal" height="150px">
            <v-card-text class="h-100 text-center">
              <!-- v-row>v-col>v-btn -> me genera las etiquetas de ambos elementos. -->
              <v-row class="h-100 mt-1">
                <v-col>
                  <v-btn
                    size="x-large"
                    :class="fan ? 'rotate' : ''"
                    :color="fan ? 'yellow' : 'blue-grey'"
                    :icon="fan ? 'mdi-fan' : 'mdi-fan-off'"
                    @click="toggleFan()"
                  ></v-btn>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>

        <!-- Slider luminosidad -->
        <v-col cols="12">
          <v-card title="Luminosidad" color="indigo-darken-3" variant="tonal" height="150px">
            <v-card-text>
              <v-row>
                <v-col class="text-center">
                  <span class="text-h2 font-weight-light">{{ luminosity }}</span>
                </v-col>
              </v-row>

              <v-slider
                v-model="luminosity"
                :step="1"
                min="0"
                max="1023"
              >
                <template v-slot:prepend>
                  <span>0</span>
                </template>

                <template v-slot:append>
                  <span>1023</span>
                </template>
              </v-slider>

            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-col>

    <!-- Sección derecha -->
    <v-col cols="3"></v-col>

    <!-- Columna 2 -->
    <v-col>
      <v-card title="Gráfico 2" color="indigo-darken-3" variant="tonal" height="400px">
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
const light = ref(false);
const fan = ref(false);
const luminosity = ref(0);

const toggleLight = () => {
  //            = !true
  //light.value = false
  // !true = false
  // !false = true
  light.value = !light.value;
}

const toggleFan = () => {
  fan.value = !fan.value;
}

onBeforeMount(() => {
  socket.connect();

  if (undefined !== localStorage.theme && 'darkMode' === localStorage.theme) {
    am4core.useTheme(am4themes_dark);
  } else {
    am4core.useTheme(am4themes_animated);
  }
});

onMounted(() => {
  socket.emit('inicio', route.params.key);

  initZoomableChart();
  initCustomChart();

  socket.on('dispositivo', (dispositivo) => {
    console.log('Dispositivo no encontrado');
  });
});

const socket = io(`http://localhost:${import.meta.env.VITE_WEBSOCKET_PORT}`, {
  autoConnect: false
});

const zoomableChart = ref(null);
const initZoomableChart = () => {
  let chart = am4core.create(zoomableChart.value, am4charts.XYChart);

  socket.on('temperatura', ({date, value}) => {
    console.log('Temperatura: ', value);
    
    chart.addData({date: new Date(date), value: value });

    if (value > 40) {
      fan.value = true;
    } else {
      fan.value = false;
    }
  });

  let dateAxis = chart.xAxes.push(new am4charts.DateAxis());
  dateAxis.renderer.grid.template.location = 0;
  dateAxis.renderer.minGridDistance = 50;

  let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());

  let series = chart.series.push(new am4charts.LineSeries());
  series.dataFields.valueY = "value";
  series.dataFields.dateX = "date";
  series.strokeWidth = 3;
  series.fillOpacity = 0.5;

  chart.scrollbarY = new am4core.Scrollbar();
  chart.scrollbarY.marginLeft = 0;

  chart.cursor = new am4charts.XYCursor();
  chart.cursor.behavior = "zoomY";
  chart.cursor.lineX.disabled = true;
}

const customChart = ref(null);
const initCustomChart = () => {
  let chart = am4core.create(customChart.value, am4charts.XYChart3D);

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

  let categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
  categoryAxis.dataFields.category = "year";
  categoryAxis.numberFormatter.numberFormat = "#";
  categoryAxis.renderer.inversed = true;

  let valueAxis = chart.xAxes.push(new am4charts.ValueAxis());

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
  height: 320px;
}

.rotate {
  animation-name: rotate;
  animation-duration: 4s;
}

@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg)
  }
}
</style>
