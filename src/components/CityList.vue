<template>
    <div v-for="city in savedCities" :key="city.id">
        <city-card :city="city" @click="goToCityView(city)"></city-card>
    </div>
</template>

<script setup>
import axios from 'axios';
import { reactive, ref } from 'vue';
import CityCard from './CityCard.vue';
import { useRouter } from 'vue-router';



var savedCities = reactive([]);

const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities = JSON.parse(localStorage.getItem('savedCities'));

        const request = [];
        savedCities.forEach((city) => {
            request.push(
                axios.get(
                    //open-meteo
                    // `https://api.open-meteo.com/v1/forecast?latitude=${city.coords.lat}&longitude=${city.coords.lon}&current_weather=true`
                    //open-weather
                    `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lon}&appid=b36e58192e852ea4b36df4490fff49c3&units=metric`

                )
            );
        });

        const weatherData = await Promise.all(request);

        weatherData.forEach((data, index) => {
            savedCities[index].weather = data;
        });
    }
}
await getCities();

//click card to go to city view
const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: {
            state: city.state,
            city: city.city
        },
        query: {
            lat: city.coords.lat,
            lon: city.coords.lon
        }
    });
}
</script>
