<template>
    <div class="flex flex-col flex-1 items-center">
        <!-- add city message -->
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
            <p>
                You are currently previewing this city, click the "+" icon to save it.
            </p>
        </div>

        <!-- weather data -->
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl font-semibold mb-2">{{ route.params.city }}</h1>
            <p class="text-6xl mb-8">
                {{ Math.round(weatherData.main.temp) }}&deg;
            </p>
            <div class="text-center">
                <p>
                    Feels like
                    {{ Math.round(weatherData.main.feels_like) }}&deg;
                </p>
                <p class="capitalize">
                    {{ weatherData.weather[0].description }}
                </p>
                <img class="w-[6rem] h-auto"
                    :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}.png`" alt="">
            </div>
        </div>

        <!-- map -->
        <MapboxMap style="height: 400px; width: 700px; margin-top: 20px; margin-bottom: 20px" :access-token=mapboxAPIkey
            map-style="mapbox://styles/mapbox/streets-v11" :center="[weatherData.coord.lon, weatherData.coord.lat]"
            :zoom="8" @mb-created="(mapboxInstance) => map = mapboxInstance">
        </MapboxMap>
    </div>

    <!-- api.open-meteo.com -->
    <!-- weather data -->
    <!-- <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-5xl font-semibold mb-2">{{ route.params.city }}</h1>
            <p class="text-8xl mb-8">
                {{ Math.round(weatherData.current_weather.temperature) }}&deg;
            </p>
            <p class="text-2xl mb-8">
                Wind speed:
                {{ Math.round(weatherData.current_weather.windspeed) }} km/h
            </p>
        </div> -->

    <!-- map -->
    <!-- <MapboxMap style="height: 400px; width: 700px; margin-top: 20px; margin-bottom: 20px" :access-token=mapboxAPIkey
            map-style="mapbox://styles/mapbox/streets-v11" :center="[weatherData.longitude, weatherData.latitude]"
            :zoom="8" @mb-created="(mapboxInstance) => map = mapboxInstance">
        </MapboxMap> -->
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

//map
import { MapboxMap } from '@studiometa/vue-mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';


const mapboxAPIkey = import.meta.env.VITE_MAP_BOX_API;
const route = useRoute();
const getWeatherData = async () => {
    try {
        //BOTH WORKING!!!
        //api.open-meteo.com
        // const weatherData = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${route.query.lat}&longitude=${route.query.lon}&current_weather=true`)
        //api.openweathermap.org
        const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lon}&appid=b36e58192e852ea4b36df4490fff49c3&units=metric`)
        return weatherData.data;
    } catch (err) {
        console.log(err)
    }

};

const weatherData = await getWeatherData();
</script>