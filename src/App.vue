<template>
  <div class="main-bg">
    <div class="wheater-app">
      <h2>Погодное приложение</h2>
      <p>Узнать погоду в {{ city == "" ? "любом городе мира" : fullName }}</p>
      <div class="inputs">
        <input
          placeholder="Введите город"
          type="text"
          v-model="city"
          :class="{ error__inp: error !== '' }"
        />
        <button @click="getWheather()" v-if="city" type="submit">
          Узнать погоду
        </button>
      </div>
      <div class="info" v-if="info != null">
        <p>{{ showTemp }} &#176;C</p>
        <p>{{ feelTemp }} &#176;C</p>
        <p>{{ minTemp }} &#176;C</p>
        <p>{{ maxTemp }} &#176;C</p>
      </div>
      <p class="error">{{ error }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },

  methods: {
    getWheather() {
      if (this.city.trim().length < 2) {
        this.error = "Слишком короткое название";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=62baa063acf53baa41db2a61d4c90034`
        )
        .then((res) => (this.info = res.data));
    },
  },
  computed: {
    fullName() {
      return '"' + this.city + '"';
    },

    showTemp() {
      return "Температура: " + this.info.main.temp;
    },
    feelTemp() {
      return "Ощущается как: " + this.info.main.feels_like;
    },
    minTemp() {
      return "Минимальная температура: " + this.info.main.temp_min;
    },
    maxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max;
    },
  },
};
</script>

<style scope lang="scss">
body {
  margin: 0;
  padding: 0;
}

h2 {
  margin: 0;
}

.main-bg {
  height: 100vh;
  display: flex;
  align-items: center;
  /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#f0f9ff+0,cbebff+47,a1dbff+100;Blue+3D+%2313 */
  background: rgb(240, 249, 255); /* Old browsers */
  background: -moz-linear-gradient(
    left,
    rgba(240, 249, 255, 1) 0%,
    rgba(203, 235, 255, 1) 47%,
    rgba(161, 219, 255, 1) 100%
  ); /* FF3.6-15 */
  background: -webkit-linear-gradient(
    left,
    rgba(240, 249, 255, 1) 0%,
    rgba(203, 235, 255, 1) 47%,
    rgba(161, 219, 255, 1) 100%
  ); /* Chrome10-25,Safari5.1-6 */
  background: linear-gradient(
    to right,
    rgba(240, 249, 255, 1) 0%,
    rgba(203, 235, 255, 1) 47%,
    rgba(161, 219, 255, 1) 100%
  ); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f0f9ff', endColorstr='#a1dbff',GradientType=1 ); /* IE6-9 */
}

.wheater-app {
  box-shadow: 2.8px 2.8px 2.2px rgba(0, 0, 0, 0.02),
    6.7px 6.7px 5.3px rgba(0, 0, 0, 0.028),
    12.5px 12.5px 10px rgba(0, 0, 0, 0.035),
    22.3px 22.3px 17.9px rgba(0, 0, 0, 0.042),
    41.8px 41.8px 33.4px rgba(0, 0, 0, 0.05),
    100px 100px 80px rgba(0, 0, 0, 0.07);
  border-radius: 18px;
  padding: 40px 24px;
  margin: auto;
  max-width: 400px;
  background: white;

  h2,
  p {
    color: #494949;
  }

  .error {
    color: red;
  }

  .inputs {
    display: flex;
    gap: 20px;
  }

  .error__inp {
    border: 2px dashed red;
    &:focus {
      border-bottom: 2px solid red;
    }
  }

  button {
    padding: 10px 20px;
    text-align: center;
    border-radius: 18px;
    border: none;
    cursor: pointer;
    transition: all 0.3s ease-in-out;
    background: #fff;
    color: rgb(36, 62, 175);
    border: 1px solid rgb(36, 62, 175);
    &:hover {
      border: 1px solid rgba(36, 61, 175, 0);
      color: #fff;
      background: rgb(36, 62, 175);
    }
  }

  input {
    border: none;
    outline: none;
    color: #494949;
    border-bottom: 2px solid rgba(36, 61, 175, 0);

    &:focus {
      border-bottom: 2px solid rgb(36, 62, 175);
    }
  }
}
</style>
