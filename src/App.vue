<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section', 'section-left', {'section-error': isError}]">
              <div class="info">
                <div class="city-inner">
                  <input 
                    v-model="city" 
                    type="text" 
                    class="search"
                    @keyup.enter="getWeather"
                  >
                </div>
                <WeatherSommary 
                  v-if="!isError"
                  :weatherInfo="weatherInfo"
                 />
                 <div v-else class="error">
                  <div class="error_title">
                    Ooops! Something went wrong
                  </div>
                  <div class="error_message">
                    {{ capitalizeFirstLetter(weatherInfo?.message) }}
                  </div>
                 </div>
              </div>
            </section>
            <section v-if="!isError" class="section section-right">
              <Highlights :weatherInfo="weatherInfo"/>
            </section> 
          </div>
          <div v-if="!isError" class="sections">
            <Coords :coord="weatherInfo.coord"/>
            <Humidity :humidity="weatherInfo.main.humidity"/>
            
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import {ref, onMounted, computed} from 'vue';
import {API_KEY, BASE_URL} from './constans'
import {capitalizeFirstLetter} from './utils'
import WeatherSommary from "./components/WeatherSommary.vue"
import Highlights from "./components/Highlights.vue"
import Coords from './components/coords.vue'
import Humidity from './components/Humidity.vue'

const city =  ref('Bar');
const weatherInfo = ref(null);
const isError = computed(() => weatherInfo.value?.cod !== 200);

function getWeather(){
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
   .then((response) => response.json())
   .then((data) => weatherInfo.value = data)
}

onMounted(getWeather)
</script>

<style lang="sass" >
  @import "assets/styles/main.sass"
  
html,
body
  margin: 0
  padding: 0
  font-family: 'Inter', Arial, sans-serif
  font-size: 16px
  line-height: 1.4
  color: $white

a
  font-size: inherit
  line-height: inherit
  color: inherit

.container
  max-width: 1200px
  width: 100%
  margin: 0 auto
  padding: 0 3%

.section
  z-index: 2
  position: relative

.block-bottom
  height: 100%
  margin-left: 10px
  padding: 28px 16px 16px
  background: url('/src/assets/img/gradient-1.jpg') no-repeat 0% 0%
  background-size: cover
  border-radius: 25px

  &-inner
    display: flex
    align-items: center


  &-pic
    width: 15%
    height: 70px
    background-size: contain
    background-repeat: no-repeat
    border-radius: 16px


  &-texts
    width: 85%
    padding-left: 20px


  &-text-block
    
    &:first-child
      padding-bottom: 12px

    &-title
      padding-bottom: 4px
      font-size: 15px
      font-weight: 700

    &-desc
      font-size: 13px
      color: rgba($white, 0.75)

.pic-humidity
  background-image: url('/src/assets/img/humidity.png')

.pic-coords
  background-image: url('/src/assets/img/compass.png')

.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #59585d

.laptop
  width: 100%
  padding: 20px
  background-color: #0e100f
  border-radius: 25px

.sections
  display: flex
  width: 100%
  flex-direction: inherit

.section-left
  width: 30%
  padding-right: 10px

  &.section-error
    min-width: 235px
    width: auto
    padding-right: 0

  .section-right
  width: 70%


.city-inner
  position: relative
  display: inline-block
  width: 100%

  &::after
    content: ''
    position: absolute
    top: 0
    right: 10px
    width: 25px
    height: 25px
    background: url('./assets/img/search.svg') no-repeat 50% 50%
    background-size: contain
    transform: translateY(50%)
    cursor: pointer

.info
  height: 100%
  padding: 16px
  background: url('./assets/img/gradient-1.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 25px

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgba(0, 0, 0, 0.75)
  border-radius: 16px
  border: none
  outline: none
  cursor: pointer

.section-bottom
  width: 50%
  margin-top: 16px

  @media (max-width: 767px)
    width: 100%

.error
 padding-top: 20px

 &_title
  font-size: 20px
  font-weight: 800

  &message
   padding-top: 20px
   font-size: 12px
</style>
