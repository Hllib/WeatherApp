<template>
  <div style="display: flex; justify-content: center">
    <div class="q-pa-md" style="max-width: 350px">
      <q-expansion-item
        class="shadow-1 overflow-hidden"
        style="border-radius: 30px"
        icon="explore"
        label="Student info"
        header-class="bg-primary text-white"
        expand-icon-class="text-white"
      >
        <q-card>
          <q-card-section>
            This app was created by Hlib Monastyrov, 122-21-1
          </q-card-section>
        </q-card>
      </q-expansion-item>
    </div>

    <div class="q-pa-md" style="max-width: 350px">
      <q-btn label="Submit" color="primary" @click="handleLondon" />
      <q-expansion-item
        class="shadow-1 overflow-hidden"
        style="border-radius: 30px"
        icon="explore"
        label="About task"
        header-class="bg-primary text-white"
        expand-icon-class="text-white"
      >
        <q-card>
          <q-card-section>
            The task was to test API calls to OpenWeather.org and create an
            application on Quasar Framework
          </q-card-section>
        </q-card>
      </q-expansion-item>
    </div>
  </div>
  <div>
    <q-splitter v-model="splitterModel" style="height: 250px">
      <template v-slot:before>
        <q-tabs v-model="tab" vertical class="text-teal">
          <q-tab name="Kyiv" icon="map" label="Kyiv" />
          <q-tab name="London" icon="map" label="London" />
          <q-tab name="Paris" icon="map" label="Paris" />
        </q-tabs>
      </template>

      <template v-slot:after>
        <q-tab-panels
          v-model="tab"
          animated
          swipeable
          vertical
          transition-prev="jump-up"
          transition-next="jump-up"
        >
          <q-tab-panel name="Kyiv">
            <div class="text-h4 q-mb-md">{{ KyivField }}</div>
          </q-tab-panel>

          <q-tab-panel name="London">
            <div class="text-h4 q-mb-md">{{ LondonField }}</div>
          </q-tab-panel>

          <q-tab-panel name="Paris">
            <div class="text-h4 q-mb-md">{{ ParisField }}</div>
          </q-tab-panel>
        </q-tab-panels>
      </template>
    </q-splitter>
  </div>

  <div class="q-px-lg q-py-md">
    <q-timeline color="secondary">
      <q-timeline-entry heading body="Inforamtion section" />

      <q-timeline-entry
        title="OpenWeather.org"
        subtitle="How do we access weather data?"
        icon="cloud"
        color="orange"
        :body="par1"
      />
      <q-timeline-entry
        title="Quasar Framework"
        subtitle="How does this website work?"
        color="orange"
        icon="info"
        :body="par2"
      />
    </q-timeline>
  </div>
</template>

<script>
import { ref } from "vue";
import { api } from "boot/axios";

export default {
  setup() {
    const cityName = ref("London");
    const KyivField = ref("Response will be displayed here");
    const LondonField = ref("Response will be displayed here");
    const ParisField = ref("Response will be displayed here");
    const data = ref(null);

    const updateCity = (newCity) => {
      cityName.value = newCity;
    };

    const updateKyivField = () => {
      KyivField.value = data.value;
    };
    const updateLondonField = () => {
      LondonField.value = data.value;
    };
    const updateParisField = () => {
      ParisField.value = data.value;
    };

    const onSubmit = (city) => {
      updateCity(city);
      api
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${cityName.value}&appid=30292a6a5232adf186142c5e87134704`
        )
        .then((response) => {
          console.log(response.data);
          data.value = response.data;
        });
    };

    return {
      par1: "OpenWeatherMap is an online service, owned by OpenWeather Ltd, that provides global weather data via API, including current weather data, forecasts, nowcasts and historical weather data. The company provides a minute-by-minute hyperlocal precipitation forecast. The convolutional machine learning model is used to utilise meteorological broadcast services and data from airport weather stations, on-ground radar stations, weather satellites, remote sensing satellites, METAR, and automated weather stations.",
      par2: "Quasar Framework is an open-source Vue.js based framework for building apps with a single codebase. It is able to be deployed on the Web as a SPA, PWA, SSR, to a Mobile App, using Cordova for iOS & Android, and to a Desktop App, using Electron for Mac, Windows, and Linux.",
      body: "Lorem ipsum",
      tab: ref("Kyiv"),
      KyivField,
      ParisField,
      data,
      LondonField,
      splitterModel: ref(20),
      handleKyiv() {
        onSubmit("Kyiv");
        updateKyivField();
      },
      handleParis() {
        onSubmit("Paris");
        updateParisField();
      },
      handleLondon() {
        onSubmit("London");
        updateLondonField();
      },
    };
  },
};
</script>
