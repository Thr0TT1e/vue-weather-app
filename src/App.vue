<template>
  <div
      id="app"
      :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : ''"
  >
    <main>
      <div class="search-box">
        <input
            v-model="query"
            @keypress="fetchWeather"
            type="text"
            class="search-bar"
            placeholder="Поиск..."
        >
      </div>
      
      <div
          class="weather-wrap"
          v-if="typeof weather.main !== 'undefined'"
      >
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      API_KEY:  '67d8a54513d0c00719d6884c4136ba51',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:    '',
      weather:  {},
    }
  },
  
  methods: {
    fetchWeather(event) {
      if (event.key === 'Enter') {
        this.fetchToAPI()
      }
    },
    
    fetchToAPI() {
      fetch(`${this.url_base}weather?q=${this.query ? this.query : 'Moscow'}&units=metric&appid=${this.API_KEY}`)
          .then(res => res.json())
          .then(this.setResults)
          .catch(err => console.log(err))
    },
    
    setResults(results) {
      this.weather = results
    },
    
    dateBuilder() {
      let d      = new Date(),
          months = ['Январь', 'Февраль', 'Март', 'Апрель',
                    'Май', 'Июнь', 'Июль', 'Август',
                    'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
          days   = ['Понедельник', 'Вторник', 'Среда', 'Четверг',
                    'Пятница', 'Суббота', 'Воскресенье'],
          day    = days[d.getDay()],
          date   = d.getDate(),
          month  = months[d.getMonth()],
          year   = d.getFullYear();
      
      return `${day} ${date} ${month} ${year}`
    },
  },
  
  mounted() {
    this.fetchToAPI()
  },
}
</script>

<style>
* {
  margin:     0;
  padding:    0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
}

#app {
  background-image:    url("./assets/cold.jpg");
  background-size:     cover;
  background-position: bottom;
  transition:          .4s;
}

#app.warm {
  background-image: url("./assets/warm.jpg");
}

main {
  min-height:       100vh;
  padding:          25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, .35), rgba(0, 0, 0, .75));
}

.search-box {
  width:         100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display:          block;
  width:            100%;
  padding:          15px;
  color:            #313131;
  font-size:        20px;
  appearance:       none;
  border:           none;
  outline:          none;
  background:       none;
  box-shadow:       0 0 8px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .5);
  border-radius:    0 16px 0 16px;
  transition:       .4s;
}

.search-box .search-bar:focus {
  box-shadow:       0 0 16px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .75);
  border-radius:    16px 0 16px 0;
}

.location-box .location {
  color:       #fff;
  font-size:   32px;
  font-weight: 500;
  text-align:  center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.location-box .date {
  color:       #fff;
  font-size:   20px;
  font-weight: 300;
  font-style:  italic;
  text-align:  center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display:          inline-block;
  padding:          10px 25px;
  color:            #ffffff;
  font-size:        102px;
  font-weight:      900;
  text-shadow:      3px 6px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .25);
  border-radius:    16px;
  margin:           30px 0;
  box-shadow:       3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather {
  color:       white;
  font-size:   48px;
  font-weight: 700;
  font-style:  italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}
</style>
