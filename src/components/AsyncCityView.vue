<template>
    <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
            <p>
                You are currently previewing this city, click the "+" icon to save it.
            </p>
        </div>
        <MapboxMap style="height: 400px; width: 700px"
            access-token="pk.eyJ1Ijoic2sxa29sYSIsImEiOiJjbGQ2cnhreDcxY3FqM29tbTlnZndhY2l1In0.fy9FCbDkCMN0YDrx08F0TQ"
            map-style="mapbox://styles/mapbox/streets-v11" :center="[weatherData.longitude, weatherData.latitude]"
            :zoom="9" @mb-created="(mapboxInstance) => map = mapboxInstance">
        </MapboxMap>

        <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
        <p class="text-8xl mb-8">
            {{ Math.round(weatherData.current_weather.temperature) }}&deg;
        </p>
        <p class="text-2xl mb-8">
            Wind:
            {{ Math.round(weatherData.current_weather.windspeed) }} km/h
        </p>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

//map
import { MapboxMap } from '@studiometa/vue-mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

const route = useRoute();
const getWeatherData = async () => {
    try {
        //api.open-meteo.com
        const weatherData = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${route.query.lat}&longitude=${route.query.lon}&current_weather=true`)
        console.log(weatherData.data)
        return weatherData.data;
    } catch (err) {
        console.log(err)
    }
};
const weatherData = await getWeatherData();
</script>