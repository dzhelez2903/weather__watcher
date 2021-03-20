<template>
  <div class="main">
    <section class="input">
      <div>
        <h1>Weather watcher</h1>
        <input
            v-model="city"
            @keydown.enter="add"
            type="text"
            placeholder= "Enter city name..."
        />
      </div>
      <button @click="add" type="button">
        Add city
      </button>
    </section>

    <section>
    <template v-if="cities.length">
      <hr/>
      <div class="city_container">
      <div class="city_border" v-for="w in cities"
           :key="w.name"
           @click="selected(w)"
           :class="{selected: sel === w}"
      >

      <div class="cityBlock" @click="add">
          {{ w.name }}, {{ w.country }}
          <div class="icon">
          <img v-bind:src="'http://openweathermap.org/img/wn/' + w.icon + '.png'">
          </div>
          {{ w.temperature }}&degC
          <a class="btn" @click.stop="handleDelete(w)">
            <svg width="34px" height="44px" viewBox="0 0 54 64" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:>
              <!-- Generator: Sketch 3.0.3 (7891) - http://www.bohemiancoding.com/sketch -->
              <desc>Created with Sketch.</desc>
              <defs></defs>
              <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" sketch:type="MSPage">
                <g id="Trash" sketch:type="MSLayerGroup" transform="translate(1.000000, 1.000000)" stroke="#fff" stroke-width="2">
                  <path d="M48,13.8 L43.9,59.2 C43.9,60.7 42.7,62 41.1,62 L10.9,62 C9.4,62 8.1,60.8 8.1,59.2 L4,13.8" id="Shape" sketch:type="MSShapeGroup"></path>
                  <path d="M8.8,13.9 L2.6,13.9 C1.2,13.9 0,12.7 0,11.3 L0,8.7 C0,7.2 1.2,6.1 2.6,6.1 L49.3,6.1 C50.7,6.1 51.9,7.3 51.9,8.7 L51.9,11.3 C51.9,12.8 50.7,13.9 49.3,13.9 L43.1,13.9" id="Shape" sketch:type="MSShapeGroup"></path>
                  <path d="M25.9,19 L25.9,54" id="Shape" sketch:type="MSShapeGroup"></path>
                  <path d="M14.9,19 L16.3,53.9" id="Shape" sketch:type="MSShapeGroup"></path>
                  <path d="M36.9,19.1 L35.6,53.9" id="Shape" sketch:type="MSShapeGroup"></path>
                  <rect id="Rectangle-path" sketch:type="MSShapeGroup" x="19" y="0" width="14" height="6"></rect>
                </g>
              </g>
            </svg>
          </a>
        </div>
      </div>
      </div>
      <hr/>
    </template>
    </section>

    <section>
      <div class="description" v-if="sel" :class="{hot: sel.temperature >= 16}">
        <div v-for="(idx) in cities" :key="idx">
          <div class="city">
            <p class="name">{{ sel.name }}, {{ sel.country }}</p>
            <p class="date">{{ dateBuilder () }}</p>
            <p class="weather">{{ sel.weather }}</p>
          </div>
          <div class="icons">
            <div class="wind">{{ sel.wind }}m/s</div>
            <div class="humidity">{{ sel.humidity }}%</div>
            <div class="temperature">{{ sel.temperature }}&degC</div>
          </div>
        </div>
        <a @click="sel = null">
          <svg width="54px" height="64px" viewBox="0 0 54 64" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:>
            <!-- Generator: Sketch 3.0.3 (7891) - http://www.bohemiancoding.com/sketch -->
            <desc>Created with Sketch.</desc>
            <defs></defs>
            <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" sketch:type="MSPage">
              <g id="Trash" sketch:type="MSLayerGroup" transform="translate(1.000000, 1.000000)" stroke="#fff" stroke-width="2">
                <path d="M48,13.8 L43.9,59.2 C43.9,60.7 42.7,62 41.1,62 L10.9,62 C9.4,62 8.1,60.8 8.1,59.2 L4,13.8" id="Shape" sketch:type="MSShapeGroup"></path>
                <path d="M8.8,13.9 L2.6,13.9 C1.2,13.9 0,12.7 0,11.3 L0,8.7 C0,7.2 1.2,6.1 2.6,6.1 L49.3,6.1 C50.7,6.1 51.9,7.3 51.9,8.7 L51.9,11.3 C51.9,12.8 50.7,13.9 49.3,13.9 L43.1,13.9" id="Shape" sketch:type="MSShapeGroup"></path>
                <path d="M25.9,19 L25.9,54" id="Shape" sketch:type="MSShapeGroup"></path>
                <path d="M14.9,19 L16.3,53.9" id="Shape" sketch:type="MSShapeGroup"></path>
                <path d="M36.9,19.1 L35.6,53.9" id="Shape" sketch:type="MSShapeGroup"></path>
                <rect id="Rectangle-path" sketch:type="MSShapeGroup" x="19" y="0" width="14" height="6"></rect>
              </g>
            </g>
          </svg>
        </a>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      api_key: '2cdac6125b9c12ea56115604b0ac04f8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      language: 'ru',
      city: '',
      cities: [],
      removeCity: [],
      details: [],
      sel: null,
    };
  },

  methods: {
    add() {
      const currentCity = {
        name: this.city,
        country: '',
        weather: '',
        temperature: '',
        wind: '',
        humidity: '',
        icon:'',
      };

      if (currentCity.name.length > 0) {
        this.cities.push(currentCity);

        setTimeout(async () => {
          const f = await fetch(
              `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
          );
          const data = await f.json();
          console.log(data);

          this.cities.find(w => w.name === currentCity.name).name = data.name;

          this.cities.find(w => w.name === currentCity.name).country = data.sys.country;

          this.cities.find(w => w.name === currentCity.name).weather = data.weather[0].description;

          this.cities.find(w => w.name === currentCity.name).temperature = Math.round(data.main.temp);

          this.cities.find(w => w.name === currentCity.name).humidity = data.main.humidity;

          this.cities.find(w => w.name === currentCity.name).wind = Math.round(data.wind.speed);

          this.cities.find(w => w.name === currentCity.name).icon = data.weather[0].icon;

          if (this.sel?.name === currentCity.name) {
            this.details.push(name);
          };
        }, 10);

        this.removeCity.push(setInterval(
                async () => {
                  const f = await fetch(
                      `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
                  );
                  const data = await f.json();
                  console.log(data);

                  this.cities.find(w => w.name === currentCity.name).name = data.name;

                  this.cities.find(w => w.name === currentCity.name).country = data.sys.country;

                  this.cities.find(w => w.name === currentCity.name).weather = data.weather[0].description;

                  this.cities.find(w => w.name === currentCity.name).temperature = Math.round(data.main.temp);

                  this.cities.find(w => w.name === currentCity.name).humidity = data.main.humidity;

                  this.cities.find(w => w.name === currentCity.name).wind = Math.round(data.wind.speed);

                  this.cities.find(w => w.name === currentCity.name).icon = data.weather[0].icon;

                  if (this.sel?.name === currentCity.name) {
                    this.details.push(name);
                  }
                }, 300000));
      }
      this.city = '';
    },

    selected(city) {
      this.sel = city;
      this.details = [];
    },

    handleDelete(cityToRemove) {
      let idx = this.cities.indexOf(cityToRemove);
      clearInterval(this.removeCity[idx]);
      this.removeCity = this.removeCity.filter(e => e !== this.removeCity[idx]);
      this.cities = this.cities.filter(w => w !== this.cities[idx]);
    },

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">

@import "assets/scss/reset.css";

@font-face {
  font-family: "Montserrat Regular";
  src: url(./assets/fonts/Montserrat-Regular.ttf) format("ttf");
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: "Montserrat-Regular", sans-serif;
}

svg {
  cursor: pointer;

  &:hover g {
    stroke: darkred;
    transition: 0.3s;
  }
}

#app {
  background-image: url("./assets/img/bg.jpg");
  background-size: cover;
  background-position: bottom;
}

.main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.input{
  margin: 0 auto;
  padding: 50px 0;
  text-align: center;
  width: 80%;

  h1 {
    font-size: 90px;
    line-height: 1;
    color: #fff;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }

  input {
    display: block;
    width: 36%;
    margin: 30px auto;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background-color: rgba(255,255,255,0.3);
    border-radius: 0 16px 0 16px;
    box-shadow: 0 0 8px rgba(0,0,0,0.25);
    transition: 0.3s;

    &:focus {
      background-color: rgba(255,255,255,0.5);
      border-radius: 16px 0 16px 0;
      box-shadow: 0 0 16px rgba(0,0,0,0.25);
    }
    &::placeholder {
      color: #313131;
    }
  }
  button {
    color: #313131;
    background-color: rgba(255,255,255,0.3);
    box-shadow: 0 0 8px rgba(0,0,0,0.25);
    border-radius: 5px;
    border: none;
    padding: 7px 35px;
    font-size: 20px;
  }
}

.selected {
  border: 3px solid #fff;
  border-radius: 16px;
  margin: 30px;
  transition: 0.3s;
}

  .city_container {
    width: 80%;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;

    .city_border {
      margin: 30px;
    }

    .cityBlock {
      width: 300px;
      padding: 30px;
      display: flex;
      flex-direction: column;
      text-align: center;
      align-items: center;
      font-size: 32px;
      font-weight: 700;
      color: #fff;
      text-shadow: 3px 5px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.15);
      border-radius: 16px;
      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      cursor: pointer;

      .icon {
        width: 40%;

        img {
          width: 100%;
        }
      }

      .btn {
        margin: 30px 0 0;
      }
    }
  }

  .description {
    width: 60%;
    margin: 40px auto;
    padding: 50px 0;
    text-align: center;
    background: url("./assets/img/cold_bg.jpg") no-repeat;
    background-size: cover;
    background-position: bottom;
    border-radius: 16px;
    box-shadow: 3px 6px rgba(0,0,0,0.25);

    .city {
      width: 30%;
      margin: 0 auto;
      padding: 50px 50px;
      color: #fff;
      text-shadow: 1px 3px rgba(0,0,0,0.25);
      background-color: rgba(255,255,255,0.15);
      border-radius: 16px;
      box-shadow: 3px 6px rgba(0,0,0,0.25);
      transition: 1s;

      .name {
        font-size: 48px;
        font-weight: 700;
      }
      .date {
        font-size: 20px;
        font-weight: 300;
        font-style: italic;
        margin: 30px 0;
      }
      .weather {
        font-size: 40px;
      }
    }

    .icons {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      margin: 70px 0;
      color: #fff;
      font-size: 40px;
      font-weight: 700;
      text-shadow: 1px 3px rgba(0,0,0,0.25);

      .wind {
        text-align: center;
        padding: 50px;
        border-radius: 16px;
        background-color: rgba(255,255,255,0.15);
        box-shadow: 3px 6px rgba(0,0,0,0.25);
      }

      .humidity {
        text-align: center;
        padding: 50px;
        border-radius: 16px;
        background-color: rgba(255,255,255,0.15);
        box-shadow: 3px 6px rgba(0,0,0,0.25);
      }

      .temperature {
        text-align: center;
        padding: 50px;
        border-radius: 16px;
        background-color: rgba(255,255,255,0.15);
        box-shadow: 3px 6px rgba(0,0,0,0.25);
      }
    }
  }

.hot {
  background-image: url("./assets/img/warm_bg.jpg");
  transition: 0.5s;
}
</style>
