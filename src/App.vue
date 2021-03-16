<template>
  <div class="main">
      <section class="input">
            <div class="">
              <h1>Weather watcher</h1>
              <input
                  v-model="city"
                  @keydown.enter="add"
                  type="text"
                  placeholder= "Enter city name..."
              />
            </div>
        <button
            @click="add"
            type="button"
        >
          Add city
        </button>
      </section>

      <template class="dashBoard" v-if="cities.length">
        <hr/>
          <div
              v-for="w in cities"
              :key="w.name"
              @click="select(w)"
          >
            <div @click="add">
              <div class="date">{{ dateBuilder () }}</div>
                {{ w.name }}
                {{ w.country }}
                {{ w.weather }}
                {{ w.temperature }}&degC
                {{ w.wind }}
              <button @click.stop="handleDelete(w)">
                Удалить
              </button>
            </div>
          </div>
        <hr/>
      </template>

      <section></section>
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
      removeCity: ''
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
      };

      if (currentCity.name.length > 0) {
        this.cities.push(currentCity);

        this.removeCity = setTimeout(async () => {
          const f = await fetch(
              `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
          );
          const data = await f.json();
          console.log(data);

          this.cities.find(w => w.name === currentCity.name).name =
              data.name;

          this.cities.find(w => w.name === currentCity.name).country =
              data.sys.country;

          this.cities.find(w => w.name === currentCity.name).weather =
              data.weather[0].main;

          this.cities.find(w => w.name === currentCity.name).temperature =
              Math.round(data.main.temp);

          this.cities.find(w => w.name === currentCity.name).wind =
              data.wind.speed;

        }, 1000);

        this.removeCity = setInterval(async () => {
          const f = await fetch(
              `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
          );
          const data = await f.json();
          console.log(data);

          this.cities.find(w => w.name === currentCity.name).name =
              data.name;

          this.cities.find(w => w.name === currentCity.name).country =
              data.sys.country;

          this.cities.find(w => w.name === currentCity.name).weather =
              data.weather[0].main;

          this.cities.find(w => w.name === currentCity.name).temperature =
              Math.round(data.main.temp);

          this.cities.find(w => w.name === currentCity.name).wind =
              data.wind.speed;

        }, 300000);
      };
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

    handleDelete(cityToRemove) {
      clearInterval(this.removeCity);
      console.log(this.cities);
      this.removeCity = '';
      this.cities = this.cities.filter(w => w !== cityToRemove);
      console.log(this.cities);
    },
  },
};
</script>

<style lang="scss">

@import "assets/scss/reset.css";

font-face {
  font-family: "Montserrat Regular";
  src: url(./assets/fonts/Montserrat-Regular.ttf) format("ttf");
  font-weight: normal;
  font-style: normal;
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
  font-family: "Montserrat-Regular", sans-serif;
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

.dashBoard {
  display: flex;
  justify-content: space-around;
  width: 80%;
  flex-wrap: wrap;
}

</style>
