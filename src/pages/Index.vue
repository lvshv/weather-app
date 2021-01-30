<template>
  <q-page class="flex flex-center">
    <div class="overlay">
      <div class="main">
        <h1 class="main-title">Прогноз погоды</h1>
        <h3 class="main-subtitle">Тренировочное приложение</h3>
      </div>
      <WeatherComponent></WeatherComponent>
    </div>
  </q-page>
</template>

<script>
import WeatherComponent from "../components/WeatherComponent.vue";
export default {
  name: "PageIndex",
  components: {
    WeatherComponent
  }
};
</script>

<style lang="scss">
.q-btn {
  font-size: 12px;
  font-weight: 400;
  line-height: 14px;
}
.q-page {
  height: 850px;
  position: relative;
  background: url("../assets/bg2.png") no-repeat center center/cover;
}

.overlay {
  content: "";
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background-color: rgba(125, 46, 185, 0.45);
}
.main-title {
  font-size: 64px;
  line-height: 75px;
  text-align: center;
  margin-top: 150px;
  margin-bottom: 90px;

  font-weight: 700;
  font-size: 64px;
  line-height: 75px;
  color: #fff;
}
.main-subtitle {
  top: 311.57px;
  font-style: normal;
  font-weight: 400;
  font-size: 24px;
  line-height: 28px;
  text-align: center;
  color: #ffffff;
  margin-bottom: 143px;
}
</style>
