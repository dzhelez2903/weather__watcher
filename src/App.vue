<template>
  <div>
    <div>
      <section>
        <div>
          <div>
            <div>
              <input
                  v-model="city"
                  @keydown.enter="add"
                  type="text"
                  name="wallet"
                  id="wallet"
                  class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                  placeholder="Например DOGE"
              />
            </div>
          </div>
        </div>
        <button
            @click="add"
            type="button"
        >
          Добавить
        </button>
      </section>

      <template v-if="cities.length">
        <hr/>

          <div
              v-for="t in cities"
              :key="t.name"
              @click="select(t)"
          >
            <div>
                {{ t.name }}
                {{ t.price }}
            </div>
            <button
                @click.stop="handleDelete(t)"
            >
            Удалить
            </button>
          </div>

        <hr/>
      </template>


      <section v-if="sel" class="relative">

      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      api_key: '2cdac6125b9c12ea56115604b0ac04f8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      language: 'ru',
      city: "",
      cities: [],
      sel: null,
    };
  },

  methods: {
    add() {
      const currentCity = {
        name: this.city,
      };

      this.cities.push(currentCity);
      setInterval(async () => {
        const f = await fetch(
            `${this.url_base}weather?q=${currentCity.name}&units=metric&APPID=${this.api_key}&lang={this.language}`
        );
        const data = await f.json();
        console.log(f);

        this.cities.find(t => t.name === currentCity.name).price =
            data.USD > 1 ? data.USD.toFixed(2) : data.USD.toPrecision(2);
      }, 10000);
      this.city = "";
    },

    handleDelete(cityToRemove) {
      this.cities = this.cities.filter(t => t !== cityToRemove);
    },
  }
};
</script>

<style> </style>
