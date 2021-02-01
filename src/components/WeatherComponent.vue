<template>
  <div class="weather">
    <div class="weather_inner">
      <div class="q-pa-md">
        <div class="q-gutter-y-md column" style="width: 260px">
          <q-input
            bottom-slots
            v-model.lazy="city"
            @input="autoSearchFilter"
            @keydown.enter="getWeather"
            label="Название"
            :error="error && error.length ? true : false"
            :rules="[val => !!val || 'Населенный пункт не найден']"
          >
            <template v-slot:append>
              <q-icon name="close" @click="city = ''" class="cursor-pointer" />
            </template>

            <q-list
              bordered
              separator
              v-if="
                autoCompleteSearch && autoCompleteSearch.length && city.length
              "
            >
              <q-item
                clickable
                v-ripple
                v-for="(item, idx) in autoCompleteSearch"
                :key="item.city + idx"
              >
                <q-item-section @click="autoFillClick(item.city)">{{
                  item.city
                }}</q-item-section>
              </q-item>
            </q-list>

            <template v-slot:hint>
              {{ error ? error : "Введите название населенного пункта" }}
            </template>
          </q-input>

          <div>
            <q-btn
              type="submit"
              unelevated
              padding="13px 49px 13px 21px"
              rounded
              color="accent"
              icon="search"
              label="Искать"
              @click="getWeather"
            />
          </div>
        </div>
      </div>
      <div class="weather_right">
        <h3 class="weather_right-title">{{ defaultCity }}</h3>
        <div class="weather_right-bottom">
          <div class="weather-bottom_left">
            <div class="weather-item">
              <span class="weather-item_title">Сегодня</span>
              <q-icon
                v-if="!weatherData.today.img"
                name="fas fa-cloud"
                size="30px"
                color="dark"
              />
              <span v-else class="weather_image"
                ><img :src="weatherData.today.img" alt=""
              /></span>
            </div>
            <div class="weather-item">
              <span class="weather-item_title">Завтра</span>
              <q-icon
                v-if="!weatherData.tomorrow.img"
                name="fas fa-cloud-rain"
                size="30px"
                color="dark"
              />
              <span v-else class="weather_image"
                ><img :src="weatherData.tomorrow.img" alt=""
              /></span>
            </div>
            <div class="weather-item">
              <span class="weather-item_title">Послезавтра</span>
              <q-icon
                v-if="!weatherData.afterTomorrow.img"
                name="fas fa-sun"
                size="30px"
                color="dark"
              />
              <span v-else class="weather_image"
                ><img :src="weatherData.afterTomorrow.img" alt=""
              /></span>
            </div>
          </div>
          <div class="weather-bottom_right">
            <div class="weather-item">
              <span class="weather-item_title">{{
                weatherData.today.text
              }}</span>
              <span class="weather-item_title">{{
                weatherData.today.temp
              }}</span>
            </div>
            <div class="weather-item">
              <span class="weather-item_title">{{
                weatherData.tomorrow.text
              }}</span>
              <span class="weather-item_title">{{
                weatherData.tomorrow.temp
              }}</span>
            </div>
            <div class="weather-item">
              <span class="weather-item_title">{{
                weatherData.afterTomorrow.text
              }}</span>
              <span class="weather-item_title">{{
                weatherData.afterTomorrow.temp
              }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import russianData from "../../russia.json";
const apiKey = "f9c312c3fd7d4ba9859123751213001";
export default {
  data: () => ({
    defaultCity: "Moscow",
    city: "",
    weatherData: {
      today: { temp: "+16", img: "", text: "Облачно" },
      tomorrow: { temp: "+21", img: "", text: "Ясно" },
      afterTomorrow: { temp: "+22", img: "", text: "Дождь" }
    },
    error: null,
    autoCompleteSearch: null
  }),
  methods: {
    async getWeather() {
      try {
        const { data } = await this.$axios.get(
          `http://api.weatherapi.com/v1/forecast.json?key=${apiKey}&q=${this.city}&days=3`
        );
        this.weatherData = {
          today: {
            temp: data.forecast.forecastday[0].day.avgtemp_c,
            img: `http:${data.forecast.forecastday[0].day.condition.icon}`,
            text: data.forecast.forecastday[0].day.condition.text
          },
          tomorrow: {
            temp: data.forecast.forecastday[1].day.avgtemp_c,
            img: `http:${data.forecast.forecastday[1].day.condition.icon}`,
            text: data.forecast.forecastday[0].day.condition.text
          },
          afterTomorrow: {
            temp: data.forecast.forecastday[2].day.avgtemp_c,
            img: `http:${data.forecast.forecastday[2].day.condition.icon}`,
            text: data.forecast.forecastday[0].day.condition.text
          }
        };
        console.log(this.weatherData);
        let newCity = this.city[0].toUpperCase() + this.city.slice(1);
        this.defaultCity = newCity;
        this.error = null;
      } catch (e) {
        this.error = "Населенный пункт не найден";
        this.$q.notify({ message: `Населенный пункт не найден`, color: "red" });
      }
    },
    autoSearchFilter(e) {
      const newArr = russianData.filter((obj, i) => {
        return obj.city.toLowerCase().includes(e.toLowerCase());
      });
      this.autoCompleteSearch = newArr;
    },
    autoFillClick(city) {
      this.city = city;
      this.autoCompleteSearch = null;
      this.getWeather();
    }
  }
};
</script>

<style lang="scss">
.q-input {
  position: relative;
}
.q-list {
  position: absolute;
  top: 55px;
  left: 0;
  right: 0;
  max-height: 400px;
  overflow-y: scroll;
}
.q-item {
  background-color: #fff;
  z-index: 5;
  max-height: 400px;
}
.weather {
  max-width: 1420px;
  height: 430px;
  background: #fff;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0px 8px 10px rgba(0, 0, 0, 0.2), 0px 2px 24px rgba(0, 0, 0, 0.14);
  border-radius: 6px;
  &_inner {
    width: 990px;
    display: flex;
    justify-content: space-between;
  }
  &_right {
    width: 430px;
  }
}

.weather_right-title {
  color: #000;
  text-align: left;
  padding: 24px 0 39px;

  font-family: Roboto;
  font-size: 29px;
  font-style: normal;
  font-weight: 300;
  line-height: 34px;
  letter-spacing: 0em;
  text-align: left;
}
.weather_right-bottom {
  display: flex;
}
.weather-bottom_right {
  min-width: 170px;
  max-width: 300px;
}
.weather-bottom_left {
  width: 220px;
  margin-right: 18px;
}
.weather-item {
  display: flex;
  justify-content: space-between;

  &_title {
    font-family: Roboto;
    font-size: 24px;
    font-style: normal;
    font-weight: 300;
    line-height: 28px;
    letter-spacing: 0em;
    text-align: left;
    color: #777;
    margin-bottom: 20px;
    white-space: nowrap;
  }
}
.weather_image {
  height: 34px;
  width: 34px;
  img {
    height: 100%;
    width: 100%;
  }
}
</style>
