<template>
    <header class="sticky-top top-0 bg-weather-primary shadow-lg">
        <nav class="flex md:flex-row sm:flex-col place-content-center text-white py-6 items-center gap-4 px-10">
            <RouterLink :to="{ name: 'home' }">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-sun fa-lg"></i>
                    <i class="text-2xl font-bold">The Local Weather</i>
                </div>
            </RouterLink>
            <div class="flex gap-4 flex-1 justify-end">
                <i @click="toggleModal"
                    class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
                <i @click="addCity"
                    class="fa-solid fa-plus  text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
            </div>
            <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
                <div class="text-black">
                    <h1 class="text-2xl mb-1">About:</h1>
                    <p class="mb-4">
                        The Local Weather allows you to track the current and
                        future weather of cities of your choosing.
                    </p>
                    <h2 class="text-2xl">How it works:</h2>
                    <ol class="list-decimal list-inside mb-4">
                        <li>
                            Search for your city by entering the name into the
                            search bar.
                        </li>
                        <li>
                            Select a city within the results, this will take
                            you to the current weather for your selection.
                        </li>
                        <li>
                            Track the city by clicking on the "+" icon in the
                            top right. This will save the city to view at a
                            later time on the home page.
                        </li>
                    </ol>

                    <h2 class="text-2xl">Removing a city</h2>
                    <p>
                        If you no longer wish to track a city, simply select
                        the city within the home page. At the bottom of the
                        page, there will be am option to delete the city.
                    </p>
                </div>
            </BaseModal>
        </nav>
    </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from 'vue-router';
import BaseModal from './BaseModal.vue';
import { reactive, ref } from 'vue';
import { uid } from 'uid';


//add city to local storage
const savedCities = reactive([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(
            localStorage.getItem('savedCities')
        );
    }


    const locationObj = {
        id: uid(),
        state: route.params.state,
        city: route.params.city,
        coords: {
            lat: route.query.lat,
            lon: route.query.lon,
        },
    };
    savedCities.push(locationObj);
    localStorage.setItem(
        "savedCities",
        JSON.stringify(savedCities)
    );
    let query = Object.assign({}, route.query);
    query.id = locationObj.id;
    delete query.preview;
    router.replace({ query });
};




//info button modal
const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
};

</script>