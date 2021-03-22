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
            :class="{error: error === '404'}"
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
            <svg class="del" width="34px" height="44px" viewBox="0 0 54 64" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:>
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
        <div class="city_block">
          <div class="city">
            <p class="name">{{ sel.name }}, {{ sel.country }}</p>
            <p class="date">{{ dateBuilder () }}</p>
            <p class="weather">{{ sel.weather }}</p>
          </div>
          <div class="icons">
            <div class="wind">
              <svg width="63px" height="48px" viewBox="0 0 63 48" version="1.1" xmlns="http://www.w3.org/2000/svg">
                <desc>Created with Sketch.</desc>
                <defs></defs>
                <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" sketch:type="MSPage">
                  <g id="Weather-wind" sketch:type="MSLayerGroup" transform="translate(0.000000, 1.000000)" stroke="#fff" stroke-width="2">
                    <path d="M34.8,8.5 C34.8,3.8 38.7,0 43.4,0 C48.2,0 52,3.8 52,8.5 C52,13.2 48.1,17 43.4,17 L4,17" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M47,30.5 C47,34.6 50.3,38 54.5,38 C58.6,38 62,34.6 62,30.5 C62,26.4 58.7,23 54.5,23 L20,23" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M27,38.5 C27,42.6 30.3,46 34.4,46 C38.5,46 41.8,42.7 41.8,38.5 C41.8,34.4 38.5,31 34.4,31 L0,31" id="Shape" sketch:type="MSShapeGroup"></path>
                  </g>
                </g>
              </svg>
              <br>
              {{ sel.wind }} m/s
            </div>
            <div class="humidity">

              <svg xmlns="http://www.w3.org/2000/svg" width="64px" height="64px"  viewBox="0 0 64 64" aria-labelledby="title"
                   aria-describedby="desc" role="img" xmlns:xlink="http://www.w3.org/1999/xlink">
                <title>Humidity</title>
                <desc>A line styled icon from Orion Icon Library.</desc>
                <path data-name="layer2"
                      d="M51.9 40.1a20.6 20.6 0 0 0-1-4.9C46.9 20.8 32 2 32 2S17.1 20.8 13 35.2a20.6 20.6 0 0 0-1 4.9c0 .5-.1 1-.1 1.5A20.2 20.2 0 0 0 32 62a20.2 20.2 0 0 0 20-20.4c0-.5 0-1-.1-1.5z"
                      fill="none" stroke="#fff" stroke-miterlimit="10" stroke-width="2" stroke-linejoin="round"
                      stroke-linecap="round"></path>
                <path data-name="layer1" fill="none" stroke="#fff" stroke-miterlimit="10"
                      stroke-width="2" d="M38 30L26 50" stroke-linejoin="round" stroke-linecap="round"></path>
                <circle data-name="layer1" cx="26" cy="32" r="4" fill="none" stroke="#fff"
                        stroke-miterlimit="10" stroke-width="2" stroke-linejoin="round" stroke-linecap="round"></circle>
                <circle data-name="layer1" cx="38" cy="48" r="4" fill="none"
                        stroke="#fff" stroke-miterlimit="10" stroke-width="2" stroke-linejoin="round"
                        stroke-linecap="round"></circle>
              </svg>
              <br>
              {{ sel.humidity }} %
            </div>
            <div class="temperature">
              <svg width="30px" height="65px" viewBox="0 0 30 65" version="1.1">
                <title>Temperature</title>
                <desc>Created with Sketch.</desc>
                <defs></defs>
                <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" sketch:type="MSPage">
                  <g id="Temperature" sketch:type="MSLayerGroup" transform="translate(0.000000, 2.000000)" stroke="#fff" stroke-width="2">
                    <path d="M22.1,42.7 L22.1,6.8 C22.1,2.1 19.1,-0.1 15.2,-0.1 C11.2,-0.1 8.2,2.1 8.2,6.8 L8.2,42.6 C5.8,44.6 4.1,47.8 4.1,51.2 C4.1,57.3 9.2,61.9 15.3,61.9 C21.4,61.9 26.2,57.3 26.2,51.2 C26.1,48 24.4,44.7 22.1,42.7 L22.1,42.7 Z" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M15.1,7 L15.1,41.8" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M21.4,47.9 C21.9,48.8 22.1,49.9 22.1,51 C22.1,54.8 19,58 15.1,58" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M25,9.9 L30,9.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M25,16.9 L28,16.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M25,23.9 L30,23.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M25,29.9 L28,29.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M25,36.9 L30,36.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M0,9.9 L5,9.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M2,16.9 L5,16.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M0,23.9 L5,23.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M2,29.9 L5,29.9" id="Shape" sketch:type="MSShapeGroup"></path>
                    <path d="M0,36.9 L5,36.9" id="Shape" sketch:type="MSShapeGroup"></path>
                  </g>
                </g>
              </svg>
              <br>
              {{ sel.temperature }} &degC
            </div>
          </div>
          <div v-for="(idx) in cities" :key="idx"></div>
        </div>
        <a @click="sel = null">
          <svg class="del" width="54px" height="64px" viewBox="0 0 54 64" version="1.1">
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
      sel: '',
      error: '200',
    };
  },

  methods: {
    add() {
      const currentCity = {
        cod: '200',
        name: this.city,
        country: '',
        weather: '',
        temperature: '',
        wind: '',
        icon: '',
        humidity: '',
      };

      if (currentCity.name.length > 0) {

        setTimeout(async () => {
          const f = await fetch(
              `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
          );
          const data = await f.json();

          currentCity.cod = data.cod;
          this.error = currentCity.cod;

          if (currentCity.cod !== '404') {
            console.log(data);

            currentCity.name = data.name;
            currentCity.country = data.sys.country;
            currentCity.weather = data.weather[0].main;
            currentCity.temperature = Math.round(data.main.temp);
            currentCity.wind = Math.round(data.wind.speed);
            currentCity.humidity = data.main.humidity;
            currentCity.icon = data.weather[0].icon;

            this.cities.push(currentCity);

            this.removeCity.push(
                setInterval(
                    async () => {
                      const f = await fetch(
                          `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
                      );
                      const data = await f.json();
                      console.log(data);
                      this.cities.find(w => w.name === currentCity.name).name = data.name;
                      this.cities.find(w => w.name === currentCity.name).country = data.sys.country;
                      this.cities.find(w => w.name === currentCity.name).weather = data.weather[0].main;
                      this.cities.find(w => w.name === currentCity.name).temperature = Math.round(data.main.temp);
                      this.cities.find(w => w.name === currentCity.name).wind = Math.round(data.wind.speed);
                      this.cities.find(w => w.name === currentCity.name).humidity = data.main.humidity;
                      this.cities.find(w => w.name === currentCity.name).icon = data.weather[0].icon;
                    }, 300000));
          }
        }, 0);
      }
      this.city = '';
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

#app {
  background-image: url("./assets/img/bg.jpg");
  background-size: cover;
  background-position: bottom;
}


.del {
  cursor: pointer;

  &:hover g {
    stroke: darkred;
    transition: 0.3s;
  }
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

.input input.error {
  border: 2px solid red;
  box-shadow: 0px 0px 27px 8px rgba(239, 32, 90, 0.2) inset;
  transition: 0.3s;
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
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        text-align: center;
        padding: 50px 70px;
        border-radius: 16px;
        background-color: rgba(255,255,255,0.15);
        box-shadow: 3px 6px rgba(0,0,0,0.25);
      }

      .humidity {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        text-align: center;
        padding: 50px 70px;
        border-radius: 16px;
        background-color: rgba(255,255,255,0.15);
        box-shadow: 3px 6px rgba(0,0,0,0.25);
      }

      .temperature {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        text-align: center;
        padding: 50px 70px;
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
