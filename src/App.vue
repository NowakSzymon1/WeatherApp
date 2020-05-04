<template>
  <div id="app" 
    :class="
    [typeof weather.main !='undefined' && weather.main.temp > 17 ? 'warm' : '', 
    typeof weather.main !='undefined' && weather.weather[0].main == 'Clouds' ? 'cloudy' : '']
     ">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Wpisz miasto np. Rio"
          v-model="query"
          @keypress="fetchWeather"
          />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
            <div class="location"> {{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ dataShow() }}</div>
        </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
            <div class="wind">Prędkość wiatru {{ Math.round(weather.wind.speed) }} m/s</div>
          </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: 'c871e82b7ef50bb3bb305e527eeff5bf',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather (e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResult);
      }
    },
    setResult (result) {
      console.log(result);
      this.weather = result;
    },
    dataShow (){
      let d = new Date();

      let days = ["Niedziela", "Poniedziałek" , "Wtorek", "Środa", "Czwartek", "Piątek", "Sobota"];
      
      let day = days[d.getDay()];

      return `${day}`;
    }
  }
}
</script>

<style>

  *{
    margin:0;
    padding: 0;
    box-sizing: border-box;

  }

  body{
    font-family: 'montserrat', sans-serif;
  }

  #app{
    background-image: url('./assets/neutral.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm{
    background-image: url('./assets/warm.jpg');
  }

  #app.cloudy{
    background-image: url('./assets/cloudy.jpg');
  }


  main{
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
  }
  
  .search-box{
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar{
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 16px 16px 16px;
  }

  .location-box .location{
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }

  .location-box .date {
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: gray;
    font-size: 100px;
    font-weight: 900;

    text-shadow: #313131;
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  .weather-box .weather {
    color: #FFF;
    font-size: 50px;
    font-weight: 700;
    font-style: italic;
  }

  .weather-box .wind{
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
  }
</style>
