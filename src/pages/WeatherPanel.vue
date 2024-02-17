<template>
  <div class="q-pa-md q-gutter-sm">
    <q-banner rounded :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
      <template v-slot:avatar>
        <img
          src="https://cdn.quasar.dev/img/mountains.jpg"
          style="width: 100px; height: 64px"
        />
      </template>

      Choose a city you like and try this yourself!
    </q-banner>
  </div>

  <div style="display: flex; justify-content: center">
    <div class="q-pa-md q-gutter-md">
      <q-card class="relative-position card-example" flat bordered>
        <q-card-section class="q-pb-none">
          <div class="text-h6">Weather</div>
        </q-card-section>

        <q-card-section>
          <transition
            appear
            enter-active-class="animated fadeIn"
            leave-active-class="animated fadeOut"
          >
            <div
              v-show="showSimulatedReturnData"
              class="text-h4 q-mb-md message-container"
              v-html="tempField"
            ></div>
          </transition>
        </q-card-section>

        <q-inner-loading :showing="visible">
          <q-spinner-gears size="50px" color="primary" />
        </q-inner-loading>
      </q-card>
    </div>

    <div class="q-pa-md" style="max-width: 400px">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="cityField"
          label="City *"
          hint="Example: London"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div>
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </div>
  </div>
</template>

<script>
import { useQuasar } from "quasar";
import { ref } from "vue";
import { api } from "boot/axios";

export default {
  setup() {
    const visible = ref(false);
    const showSimulatedReturnData = ref(false);
    const $q = useQuasar();
    const cityField = ref(null);
    const data = ref(null);
    const cityName = ref("London");

    const tempField = ref("Response will be displayed here");

    const updateCity = (newCity) => {
      cityName.value = newCity;
    };

    const updateTempField = (newField) => {
      tempField.value = newField;
    };

    return {
      visible,
      showSimulatedReturnData,
      tempField,
      cityField,
      data,
      cityName,
      onSubmit() {
        updateCity(cityField.value);
        api
          .get(
            `https://api.openweathermap.org/data/2.5/weather?q=${cityName.value}&appid=30292a6a5232adf186142c5e87134704`
          )
          .then((response) => {
            data.value = response.data;
            const responseMessage = `
    <div style="font-family: Arial, sans-serif; padding: 10px; border-radius: 5px; background-color: #f0f0f0; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); font-size: 14px; line-height: 1.2;">
      <p><strong style="color: #333; font-size: 14px;">City:</strong> <span style="color: #555;">${
        data.value.name
      }</span></p>
      <p><strong style="color: #333; font-size: 14px;">Description:</strong> <span style="color: #555;">${
        data.value.weather[0].description
      }</span></p>
      <p><strong style="color: #333; font-size: 14px;">Temperature:</strong> <span style="color: #e74c3c;">${Math.round(
        data.value.main.temp - 273.15
      )}°C</span></p>
      <p><strong style="color: #333; font-size: 14px;">Feels like:</strong> <span style="color: #e74c3c;">${Math.round(
        data.value.main.feels_like - 273.15
      )}°C</span></p>
      <p><strong style="color: #333; font-size: 14px;">Humidity:</strong> <span style="color: #3498db;">${
        data.value.main.humidity
      }%</span></p>
      <p><strong style="color: #333; font-size: 14px;">Pressure:</strong> <span style="color: #3498db;">${
        data.value.main.pressure
      } hPa</span></p>
      <p><strong style="color: #333; font-size: 14px;">Wind Speed:</strong> <span style="color: #27ae60;">${
        data.value.wind.speed
      } m/s</span></p>
      <p><strong style="color: #333; font-size: 14px;">Sunrise:</strong> <span style="color: #f39c12;">${new Date(
        data.value.sys.sunrise * 1000
      ).toLocaleTimeString()}</span></p>
      <p><strong style="color: #333; font-size: 14px;">Sunset:</strong> <span style="color: #f39c12;">${new Date(
        data.value.sys.sunset * 1000
      ).toLocaleTimeString()}</span></p>
    </div>
  `;
            updateTempField(responseMessage);
          });

        $q.notify({
          color: "green-4",
          textColor: "white",
          icon: "cloud_done",
          message: "Processing",
        });

        visible.value = true;
        showSimulatedReturnData.value = false;

        setTimeout(() => {
          visible.value = false;
          showSimulatedReturnData.value = true;
        }, 3000);
      },
      onReset() {
        cityField.value = null;
      },
    };
  },
};
</script>

<style lang="sass" scoped>
.card-example
  width: 500px
  height: 400px
</style>
