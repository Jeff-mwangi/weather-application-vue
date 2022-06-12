<template>
  <div class="weather" :class=" currentTemp >18 ? 'warm':''">
    <div class="weather__header">
      <h1 class="weather__title">WEATHER</h1>
      <input type="search"
       @keypress="getWeather" 
       name="search"
       v-model="city"
        placeholder="Search...">
      <div class="weather_header_location">
      <div class="weather_location">
        <span>{{ location }}, {{country}}</span>
      </div>
      <div class="weather_date">
        <span>{{ date }}</span>
      </div>
      </div>
    </div>
    <div class="weather__body">
      <div class="weather__body__temp">
        <div class="weather__body__temp__current">
          <span>{{ currentTemp }}</span>
          <span>°C</span>
        </div>
      </div>
      <div class="weather__body__description">
        <span>{{ description }}</span>
      </div>
    </div>
    <div class="weather__footer">
      <p>Designed by <a href="https://jeff-mwangi-portofolio.netlify.app" target="_">Geoffrey Mwangi</a></p>
    </div>
  </div>

</template>

<script>
export default {
  name: 'Weather',
  data() {
    return {

      currentTemp: '',
      minTemp: '',
      maxTemp: '',
      description: '',
      location: '',
      country: '',
      date: '',
      appid: '6a1a24893108fb7dee985b31cd47d9f6',
      city:''
    }
  },
  methods: {
    displayWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=Nairobi&appid=${this.appid}`
      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.currentTemp = Math.round(data.main.temp - 273.15)
          this.minTemp = Math.round(data.main.temp_min - 273.15)
          this.maxTemp = Math.round(data.main.temp_max - 273.15)
          this.description = data.weather[0].main
          this.location = data.name
          this.country = data.sys.country
          this.date = new Date(data.dt * 1000).toLocaleDateString()
        })
    },
   getWeather(e) {
    if(e.key == "Enter") {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.appid}`
      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.currentTemp = Math.round(data.main.temp - 273.15)
          this.minTemp = Math.round(data.main.temp_min - 273.15)
          this.maxTemp = Math.round(data.main.temp_max - 273.15)
          this.description = data.weather[0].main
          this.location = data.name
          this.country = data.sys.country
          this.date = new Date(data.dt * 1000).toLocaleDateString()
          if (data.cod == "404") {
          alert("City not found")
        }
        })
        
    }
    }

  },
  
  mounted() {
    this.displayWeather()
  },
}

</script>

<style scoped>
.weather {
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.75)), url('../assets/clouds.gif');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
.warm{
background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.75)), url('../assets/sun-rise.jpg');
}
.weather__header h1 {
  font-size: 3rem;
  font-weight: 600;
  margin: 0;
  padding: 0;
}
.weather__header input {
  margin-top:2rem;
  width: 100%;
  height: 3rem;
  border: none;
  border-radius: 15px 0 15px 0;
  background-color:rgba(255, 255, 255, 0.5);
  padding: 0 1rem;
  font-size: 1.5rem;
  font-weight: 600;
  color: #2c3e50;
  outline: none;
  box-shadow: 0px 0px 8px  rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}
.weather__header input:focus {
  border-radius: 0 15px 0 15px;
  background-color:rgba(255, 255, 255, 0.75);
  color: #2c3e50;
}
.weather__header input:hover {
  border-radius: 0 15px 0 15px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}
.weather_header_location{
  padding-top:30px;
}
.weather_location{
  font-size: 2.2rem;
  font-weight: 500;
}
.weather_date{
  font-style: italic;
  font-size:1.3rem;
  font-weight: 300;
}
.weather__body {
  margin-top: 3rem;
  font-size: 2rem;
}
.weather__body__temp__current{
  font-size: 6rem;
  font-weight: 900;
  margin-bottom: 1rem;
  padding:10px 25px;
  text-shadow: 3px 6px rgba(0, 0,0,0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius:10px;

}
.weather__body__description{
  font-size: 2rem;
  font-weight: 600;
  margin-bottom: 1rem;
  padding:10px 25px;
  text-shadow: 3px 6px rgba(0, 0,0,0.25);
}
.weather__footer{
  font-size: 1rem;
  margin-top: 2rem;
  padding: 0 1rem;
}
.weather__footer p a{
  color: #fff;
  text-decoration-line: none;
}
.weather__footer p a:hover{
  color:peru;
}


</style>