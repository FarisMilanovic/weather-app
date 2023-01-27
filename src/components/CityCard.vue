<template>
    <div class="flex py-8 pt-12 px-4 bg-weather-secondary rounded-md cursor-pointer">
        <div class="flex flex-col flex-1">
            <h2 class="text-4xl"> {{ city.city }}</h2>
            <h3 class="text-xl">{{ city.weather.data.sys.country }}</h3>
        </div>
        <div class="flex flex-col gap-2 items-center align-items place-content-center px-2">
            <p class="text-5xl">
                {{ Math.round(city.weather.data.main.temp) }}&deg;
            </p>
            <div class="flex gap-1">
                <span class="text-[15px]">
                    H:
                    {{ Math.round(city.weather.data.main.temp_max) }}&deg;
                </span>

                <span class="text-[15px]">
                    L:
                    {{ Math.round(city.weather.data.main.temp_min) }}&deg;
                </span>
            </div>
        </div>
    </div>
</template>

<script setup>

defineProps({
    city: {
        type: Object,
        required: () => { },
    }
});


</script>