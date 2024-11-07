<script setup>
  import { ref } from 'vue'
  import { apiKey } from '@/key';

  const location = ref('')

  const temperatureC = ref(0)
  const temperatureF = ref(0)
  const description = ref('')
  const cityName = ref('')
  const cityTime = ref('')
  const country = ref('')
  const locationArea = ref('')

  async function getWeather(location){
    var search = location
    const url = `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${search}`
    const response = await fetch(url)
    if(!response.ok){
      console.log('City not found.')
    } else{
      document.getElementById('meteo').style.display = 'block';
      const data = await response.json();
      temperatureC.value = data.current.temp_c;
      temperatureF.value = data.current.temp_f;
      description.value = data.current.condition.text;
      cityName.value = data.location.name;
      cityTime.value = data.location.localtime;
      country.value = data.location.country;
      locationArea.value = data.location.region;
    }
  }
</script>

<template>
  <div class="select-none mt-[4vh] w-max mx-auto text-zinc-900">
    <h1 class="w-max mx-auto mb-[2vh] text-[2vh] dm-sans font-medium bg-sky-500 drop-shadow-lg text-white px-[1.2vh] py-[0.4vh] rounded-[1.2vh]">easyWeather</h1>
    <div class="flex justify-between mt-[1vh]">
      <input id="searchBar" placeholder="Search for a location" v-model="location" class="dm-sans mx-auto w-[40vh] bg-zinc-50/75 duration-200 focus:bg-zinc-50 drop-shadow-lg rounded-[2vh] outline-none px-[1.5vh] h-[5vh] text-zinc-900">
      <button v-on:click="getWeather(location), location = ''" id="searchButton" class="h-[5vh] dm-sans bg-zinc-50/75 duration-200 hover:bg-zinc-50 active:bg-white rounded-[2vh] ml-[1vh] w-[5vh] drop-shadow-lg"></button>
    </div>
  </div>

  <div id="meteo">
    <div class="flex justify-center mt-[4vh]">
      <div class="dm-sans select-none mx-[1vh] lg:w-[62vh] w-[46vh] lg:px-[0vh] px-[4vh] text-center p-[1.5vh] bg-zinc-50 rounded-[2vh] drop-shadow-lg">
        <h1 class="space-mono lg:text-[3.2vh] text-[3vh]">{{ cityName }}</h1>
        <div class="flex w-max justify-center mx-auto">
          <h1 v-if="country">{{ country }}</h1>
          <h1 class="" v-if="locationArea">, {{ locationArea }}.</h1>
        </div>
        <h1 class="dm-sans font-medium lg:text-[2.2vh] text-[2vh] mt-[1vh] text-zinc-400">{{ cityTime }}</h1>
      </div>
    </div>
    <div class="flex justify-center mt-[2vh]">
      <div class="dm-sans select-none mx-[1vh] lg:w-[30vh] w-[22vh] text-right p-[1.5vh] bg-zinc-50 rounded-[2vh] drop-shadow-lg">
        <h1 class="space-mono text-[2.5vh] lg:text-[3vh]">{{ temperatureC }} °C</h1>
        <h1 class="dm-sans font-medium lg:text-[2vh] text-[1.8vh] text-zinc-400">Actual Temperature</h1>
      </div>
      <div class="dm-sans select-none mx-[1vh] lg:w-[30vh] w-[22vh] text-left p-[1.5vh] bg-zinc-50 rounded-[2vh] drop-shadow-lg">
        <h1 class="space-mono text-[2.5vh] lg:text-[3vh]">{{ temperatureF }} °F</h1>
        <h1 class="dm-sans font-medium lg:text-[2vh] text-[1.8vh] text-zinc-400">Actual Temperature</h1>
      </div>
    </div>
  </div>

  <div class="mt-[10vh] w-max mx-auto text-center lg:max-w-[80vh] max-w-[44vh]">
    <h2 class="dm-sans text-[1.5vh] text-zinc-500">Ho creato questo sito perchè<br>quando apro gli altri non capisco niente per i primi 30 secondi.<br>Adesso posso leggere cosa mi interessa nella meta' della meta' del tempo.</h2>
  </div>

</template>