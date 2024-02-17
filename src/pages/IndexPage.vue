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
              <div
                class="text-h4 q-mb-md message-container"
                v-html="KyivField"
              ></div>
            </q-tab-panel>

            <q-tab-panel name="London">
              <div
                class="text-h4 q-mb-md message-container"
                v-html="LondonField"
              ></div>
            </q-tab-panel>

            <q-tab-panel name="Paris">
              <div
                class="text-h4 q-mb-md message-container"
                v-html="ParisField"
              ></div>
            </q-tab-panel>
          </q-tab-panels>
        </template>
      </q-splitter>
    </div>
    <q-btn
      push
      color="white"
      text-color="primary"
      label="Load"
      @click="onSubmit"
    />
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
    const KyivField = ref("Click 'Load' to get weather info in Kyiv!");
    const LondonField = ref("Click 'Load' to get weather info in London!");
    const ParisField = ref("Click 'Load' to get weather info in Paris!");

    const updateKyivField = (field) => {
      KyivField.value = field;
    };
    const updateLondonField = (field) => {
      LondonField.value = field;
    };
    const updateParisField = (field) => {
      ParisField.value = field;
    };

    const getMessageFromResponse = (data) => {
      return `
    <div style="font-family: Arial, sans-serif; padding: 10px; border-radius: 5px; background-color: #f0f0f0; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); font-size: 14px; line-height: 1.2;">
      <p><strong style="color: #333; font-size: 14px;">City:</strong> <span style="color: #555;">${
        data.name
      }</span></p>
      <p><strong style="color: #333; font-size: 14px;">Description:</strong> <span style="color: #555;">${
        data.weather[0].description
      }</span></p>
      <p><strong style="color: #333; font-size: 14px;">Temperature:</strong> <span style="color: #e74c3c;">${Math.round(
        data.main.temp - 273.15
      )}°C</span></p>
      <p><strong style="color: #333; font-size: 14px;">Feels like:</strong> <span style="color: #e74c3c;">${Math.round(
        data.main.feels_like - 273.15
      )}°C</span></p>
      <p><strong style="color: #333; font-size: 14px;">Humidity:</strong> <span style="color: #3498db;">${
        data.main.humidity
      }%</span></p>
      <p><strong style="color: #333; font-size: 14px;">Pressure:</strong> <span style="color: #3498db;">${
        data.main.pressure
      } hPa</span></p>
      <p><strong style="color: #333; font-size: 14px;">Wind Speed:</strong> <span style="color: #27ae60;">${
        data.wind.speed
      } m/s</span></p>
      <p><strong style="color: #333; font-size: 14px;">Sunrise:</strong> <span style="color: #f39c12;">${new Date(
        data.sys.sunrise * 1000
      ).toLocaleTimeString()}</span></p>
      <p><strong style="color: #333; font-size: 14px;">Sunset:</strong> <span style="color: #f39c12;">${new Date(
        data.sys.sunset * 1000
      ).toLocaleTimeString()}</span></p>
    </div>
  `;
    };
    return {
      par1: "OpenWeatherMap is an online service, owned by OpenWeather Ltd, that provides global weather data via API, including current weather data, forecasts, nowcasts and historical weather data. The company provides a minute-by-minute hyperlocal precipitation forecast. The convolutional machine learning model is used to utilise meteorological broadcast services and data from airport weather stations, on-ground radar stations, weather satellites, remote sensing satellites, METAR, and automated weather stations.",
      par2: "Quasar Framework is an open-source Vue.js based framework for building apps with a single codebase. It is able to be deployed on the Web as a SPA, PWA, SSR, to a Mobile App, using Cordova for iOS & Android, and to a Desktop App, using Electron for Mac, Windows, and Linux.",
      body: "Lorem ipsum",
      tab: ref("Kyiv"),
      KyivField,
      ParisField,
      LondonField,
      splitterModel: ref(20),
      onSubmit() {
        api
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${"Kyiv"}&appid=30292a6a5232adf186142c5e87134704`
          )
          .then((response) => {
            console.log(response.data);
            updateKyivField(getMessageFromResponse(response.data));
          });
        api
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${"London"}&appid=30292a6a5232adf186142c5e87134704`
          )
          .then((response) => {
            console.log(response.data);
            updateLondonField(getMessageFromResponse(response.data));
          });
        api
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${"Paris"}&appid=30292a6a5232adf186142c5e87134704`
          )
          .then((response) => {
            console.log(response.data);
            updateParisField(getMessageFromResponse(response.data));
          });
      },
    };
  },
};
</script>

<style>
.message-container {
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom, #3498db, #2980b9);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

.message-container p {
  margin: 10px 0;
  color: #fff; /* Text color for paragraphs */
}

.message-container strong {
  font-weight: bold;
}

.message-container .highlight {
  color: #ffeb3b; /* Highlighted text color */
}

/* Additional styling for specific elements */
.message-container .temperature {
  color: #f39c12; /* Temperature text color */
}

.message-container .description {
  font-style: italic; /* Italicize the description */
}
</style>
