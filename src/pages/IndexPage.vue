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
            <div class="text-h4 q-mb-md">Weather in Kyiv</div>
            <p>Weather in Kyiv</p>
          </q-tab-panel>

          <q-tab-panel name="London">
            <div class="text-h4 q-mb-md">Weather in London</div>
            <p>Weather in London</p>
          </q-tab-panel>

          <q-tab-panel name="Paris">
            <div class="text-h4 q-mb-md">Weather in Paris</div>
            <p>Weather in Paris</p>
          </q-tab-panel>
        </q-tab-panels>
      </template>
    </q-splitter>
  </div>

  <div class="q-px-lg q-py-md">
    <q-timeline color="secondary">
      <q-timeline-entry heading body="What is this website?" />

      <q-timeline-entry
        title="OpenWeather.com"
        subtitle="How do we access weather data?"
        avatar="~assets/Avatar.jpg"
        :body="par1"
      />
      <q-timeline-entry
        title="Event Title"
        subtitle="February 22, 1986"
        color="orange"
        icon="done_all"
        :body="body"
      />
    </q-timeline>
  </div>
</template>

<script>
import { ref } from "vue";
import { api } from "boot/axios";

export default {
  setup() {
    const data = ref(null);

    function loadData() {
      api
        .get(
          "https://api.openweathermap.org/data/2.5/weather?lat=44.34&lon=10.99&appid=30292a6a5232adf186142c5e87134704"
        )
        .then((response) => {
          data.value = response.data;
          console.log(response.data);
        })
        .catch(() => {
          $q.notify({
            color: "negative",
            position: "top",
            message: "Loading failed",
            icon: "report_problem",
          });
        });
    }

    return {
      par1: "We use OpenWeather.com",
      par2: "",
      body: "Lorem ipsum",
      tab: ref("Kyiv"),
      splitterModel: ref(20),
      data,
      loadData,
    };
  },
};
</script>
