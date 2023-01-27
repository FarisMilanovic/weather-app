<template>
    <main class="p-[2rem] place-content-center text-white">
        <div class="pt-4 mb-8 relative">
            <input class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary 
                focus:outline-none focus:shadow-[1px_2px_0_0_#004E71]" type="text" v-model="searchQuery"
                placeholder="Search for a city or state..." @input="getSearchResults">
            <ul v-if="mapboxSeachResults"
                class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
                <p v-if="searchError" class="py-2">Sorry, something went wrong, please try again.</p>
                <p v-if="!searchError && mapboxSeachResults.length === 0">No results match your query.</p>
                <template v-else>
                    <li v-for="searchResult in mapboxSeachResults" :key="searchResult.id" class="py-2 cursor-pointer"
                        @click="previewCity(searchResult)">
                        {{ searchResult.place_name }}
                    </li>
                </template>
            </ul>
        </div>
        <div class="flex flex-col gap-4">
            <Suspense>
                <city-list>
                    <template #fallback>
                        <p>Loading...</p>
                    </template>
                </city-list>
            </Suspense>
        </div>
    </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import CityList from '../components/CityList.vue';


//preview a city
const router = useRouter();
const previewCity = (searchResult) => {
    const [city, state] = searchResult.place_name.split(",");
    router.push({
        name: "cityView",
        params: {
            city: city,
            state: state.replaceAll(" ", "")
        },
        query: {
            lat: searchResult.geometry.coordinates[1],
            lon: searchResult.geometry.coordinates[0],
            preview: true
        }
    });
};

//seach for a city
const mapboxAPIkey = import.meta.env.VITE_MAP_BOX_API;
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSeachResults = ref(null);
const searchError = ref(null);
const getSearchResults = () => {
    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout(async () => {
        if (searchQuery.value !== "") {
            try {
                const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIkey}`);
                mapboxSeachResults.value = result.data.features;
            } catch (error) {
                searchError.value = true;
            }
            return;
        }
        mapboxSeachResults.value = null;
    }, 300);
};

</script>
