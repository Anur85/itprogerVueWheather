<script>
import axios from 'axios';
export default {
  data() {
    return {
      city: '',
      error: '',
      info: null
    };
  },
  computed: {
    cityName() {
      return '«' + this.city + '»';
    },
    showTemp() {
      return 'Температура: ' + this.info.main.temp;
    },
    showFeelsLike() {
      return 'Ощущается как: ' + this.info.main.feels_like;
    },
    showMinTemp() {
      return 'Минимальная температура: ' + this.info.main.temp_min;
    },
    showMaxTemp() {
      return 'Максимальная температура: ' + this.info.main.temp_max;
    }
  },
  methods: {
    getWheather() {
      if (this.city.trim().length < 2) {
        this.error = 'Нужно название более одного символа :)';
        this.info = null;
        return false;
      }
      this.error = '';
      axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3f6f4b260e636277412b4c804d0bf93b`)
        .then((res) => (this.info = res.data))
        .catch(() => (this.info = null));
    }
  }
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодные условия</h1>
    <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-if="city != ''" @click="getWheather()">Получить данные о погоде</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>
    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: rgb(150, 10, 10);
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(255, 115, 0);
  text-align: center;
  color: rgb(226, 7, 171);
}
.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(255, 115, 0);
  color: antiquewhite;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}
.wrapper input:focus {
  border-bottom-color: aqua;
}
.wrapper button:disabled {
  background: #4e3f12;
  cursor: not-allowed;
}
.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transform: transform 500ms ease;
}
.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
