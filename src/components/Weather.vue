<template>
  <div class="weather" v-if="weatherData.adCode.city && weatherData.weather.weather">
    <span>{{ weatherData.adCode.city }}&nbsp;</span>
    <span>{{ weatherData.weather.weather }}&nbsp;</span>
    <span>{{ weatherData.weather.minTemperature }}℃ - {{ weatherData.weather.maxTemperature }}℃</span>
    <span class="sm-hidden">
      &nbsp;{{
        weatherData.weather.winddirection?.endsWith("风")
          ? weatherData.weather.winddirection
          : weatherData.weather.winddirection + "风"
      }}&nbsp;
    </span>
    <span class="sm-hidden">{{ weatherData.weather.windpower }}&nbsp;级</span>
  </div>
  <div class="weather" v-else>
    <span>天气数据获取失败，请重进网页</span>
  </div>
</template>

<script setup>
import { getAdcode, getWeather, getOtherWeather } from "@/api";
import { Error } from "@icon-park/vue-next";
import { reactive, onMounted } from 'vue';

const mainKey = import.meta.env.VITE_WEATHER_KEY;

const weatherData = reactive({
  adCode: {
    city: null,
    adcode: null,
  },
  weather: {
    weather: null,
    minTemperature: null, // 最低气温
    maxTemperature: null, // 最高气温
    winddirection: null,
    windpower: null,
  },
});

const getWeatherData = async () => {
  try {
    if (!mainKey) {
      const result = await getOtherWeather();
      const data = result.result;
      weatherData.adCode.city = data.city.City || "未知地区";
      weatherData.weather = {
        weather: data.condition.day_weather,
        minTemperature: data.condition.min_degree,
        maxTemperature: data.condition.max_degree,
        winddirection: data.condition.day_wind_direction,
        windpower: data.condition.day_wind_power,
      };
    } else {
      const adCode = await getAdcode(mainKey);
      if (adCode.infocode !== "10000") throw "地区查询失败";
      const weatherResult = await getWeather(mainKey, adCode.adcode);
      weatherData.weather = {
        weather: weatherResult.lives[0].weather,
        minTemperature: weatherResult.lives[0].temperature,
        maxTemperature: weatherResult.lives[0].temperature,
        winddirection: weatherResult.lives[0].winddirection,
        windpower: weatherResult.lives[0].windpower,
      };
    }
  } catch (error) {
    console.error("天气信息获取失败:" + error);
    onError("天气信息获取失败");
  }
};

const onError = (message) => {
  ElMessage({
    message,
    icon: h(Error, {
      theme: "filled",
      fill: "#efefef",
    }),
  });
  console.error(message);
};

onMounted(() => {
  getWeatherData();
});
</script>
