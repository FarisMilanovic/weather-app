<template>
    <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
            <p>
                You are currently previewing this city, click the "+" icon to save it.
            </p>
        </div>
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <!-- <p class="text-sm mb-12">
                {{
                    new Date(weatherData.currentTime).toLocaleDateString(
                        "en-us",
                        {
                            weekday: 'short',
                            day: '2-digit',
                            month: 'long'
                        }
                    )
                }}
                {{
    new Date(weatherData.currentTime).toLocaleDateString(
        "en-us",
        {
            timeStyle: 'short',
        }
    )
                }}
            </p> -->
            <p class="text-8xl mb-8">
                {{ Math.round(weatherData.current_weather.temperature) }}&deg;
            </p>
            <!-- <p>
                Feels like
                {{ Math.round(weatherData?.current.feels_like) }}&deg;
            </p> -->
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
    try {
        // const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude=daily&appid=b36e58192e852ea4b36df4490fff49c3`);
        const weatherData = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${route.query.lat}&longitude=${route.query.lon}&current_weather=true`)
        console.log(weatherData)
        return weatherData.data;
    } catch (err) {
        console.log(err)
    }
};
const weatherData = await getWeatherData();
</script>