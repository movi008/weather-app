<template>
    <div>
        <section class="vh-100" style="background-color: #f5f6f7;">
            <div class="container py-5 h-100">
                <div class="row d-flex justify-content-center align-items-center h-100">
                    <div class="col-md-10 col-lg-8 col-xl-6">

                        <div class="card bg-dark text-white">
                            <div class="bg-image" style="border-radius: 35px;">
                                <img src="../assets/img/nature-colorful-cloud.jpg" class="card-img" alt="weather" />
                                <div class="mask" style="background-color: rgba(190, 216, 232, .5);"></div>
                            </div>

                            <div class="card-img-overlay text-dark p-5">
                                <input type="search" class="w-75 rounded" @keyup.enter="weather_data()" v-model="inpCity"
                                    placeholder="Enter City Name" aria-label="Search" aria-describedby="search-addon" />
                                <a href="#!" @click="weather_data" type="button">
                                    <span class="input-group-text border-0 fw-bold" id="search-addon">
                                        Check!
                                    </span>
                                </a>
                                
                                <h4 class="mb-0 mt-3"> {{ state.city }} </h4>
                                <p class="display-2 my-3"> {{ state.temp }}°C</p>
                                <p class="mb-2">Feels Like: <strong>{{ state.feels }} °C</strong></p>
                                <h5 class="text-capitalize"> {{ state.descrition }} </h5>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
const inpCity = ref('khulna')
import axios from 'axios';
let state = reactive({
    city: 'Khulna, BD',
    temp: '26.21',
    feels: '26.21',
    descrition: 'Light Rain'
});


async function weather_data() {
    await axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + inpCity.value + '&appid=27bcc252742830381afd6856832da01a&lang=en&units=metric')
        .then(response => {

            state.city = response.data.name + ', ' + response.data.sys.country;
            state.temp = response.data.main.temp;
            state.feels = response.data.main.feels_like;
            state.descrition = response.data.weather[0].description;

        })
        .catch(function (e) {
            if (e.response.status == 404) {
                alert('Mistaked city name')
            }
        });
}

onMounted(() => {
    weather_data()
})


</script>

<style scoped></style>
