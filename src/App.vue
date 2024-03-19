<script>
	import axios from 'axios';
	export default {
		data() {
			return {
				city: "",
				error: "",
				info: null
			}
		},
		computed: {
			cityName() {
				return "«" + this.city + "»"
			},
      showTemp() {
        return "Температура: " + this.info.main.temp
      },
      showFeelsLike() {
        return "Ощущается как: " + this.info.main.feels_like
      },
      showMinTemp() {
        return "Минимальная температура: " + this.info.main.temp_min
      },
      showMaxTemp() {
        return "Максимальная температура: " + this.info.main.temp_max
      },
		},
		methods: {
			getWeather() {
				if(this.city.trim().length < 2) {
					this.error = "Нужно название более одного символа :)"
					return false
				}
				this.error = ""
				axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=6b3a1f6ad9c72f0643a901f4d2692178&units=metric`)
					.then(res => (this.info = res.data))
			}
		}
	}
</script>

<template>
	<div class="wrapper">
		<h1>Погодное приложение</h1>
		<p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
		<input type="text" v-model="city" placeholder="Введите город">
		<button v-if="city != ''" @click="getWeather()">Получить погоду</button>
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
	.wrapper {
		width: 900px;
		height: 500px;
		border-radius: 50px;
		background-color: rgb(5, 29, 107);
		padding: 40px;
		text-align: center;
	}

	h1 {
		font-size: 42px;
		margin-top: 40px;
	}

	p {
		margin-top: 20px;
		font-size: 28px;
		color: rgba(255, 255, 255, 0.679);
		font-weight: 300;
	}

	input {
		margin-top: 30px;
		background-color: transparent;
		border: 0;
		border-bottom: solid 2px grey;
		padding: 10px;
		color: azure;
		font-size: 16px;
		outline: none;
		font-weight: 200;
	}
	input:focus {
		border-bottom-color: white;
	}

	button {
		background-color: #0ddaed;
		font-size: 16px;
		cursor: pointer;
		padding: 12px 15px;
		margin-left: 20px;
		border-radius: 10px;
		border: none;
		transition: background-color 500ms ease;
	}
	button:hover {
		background-color: #89f1fb;
	}
	button:disabled {
		background-color: #99b5b8;
		cursor: not-allowed;
	}

	.error {
		font-size: 18px;
		color: rgb(248, 51, 51);
		margin-top: 30px;
	}
</style>