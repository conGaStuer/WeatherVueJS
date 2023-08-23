<template>
  <div class="weather__app">
    <div class="weather__app--container">
      <div class="search__bar">
        <input type="text" placeholder="Enter your City" v-model="city" />
        <button class="search_btn" @click="load">&#128269;</button>
      </div>
      <div class="error" v-if="error">
        {{ error }}
      </div>
      <div class="body">
        <div class="temp__bar">
          <div class="temp">{{ temp ? `${temp}&deg;F` : "" }}</div>
          <div class="description">{{ description }}</div>
        </div>
        <div class="day__bar">
          <div class="day">Mon day</div>
          <div class="day__info">
            <div class="temp_day">{{ temp ? `${temp}&deg;F` : "&deg;F" }}</div>
            <div class="main">{{ main ? `${main}` : "Cloudy" }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  components: {},
  setup() {
    const url = ref();
    const error = ref(null);
    const city = ref("");
    const temp = ref("");
    const description = ref("");
    const main = ref("");
    const load = async () => {
      try {
        let data = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=12d4e318447197a64a229ae21142a38c`
        );
        url.value = await data.json();
        if (city.value === url.value.name) {
          console.log(url.value.name);
          temp.value = url.value.main.temp;
          description.value = url.value.weather[0].description;
          main.value = url.value.weather[0].main;
          error.value = null;
        } else {
          error.value = "No result...";
        }
      } catch (err) {}
    };

    return {
      url,
      load,
      city,
      error,
      temp,
      description,
      main,
    };
  },
};
</script>
