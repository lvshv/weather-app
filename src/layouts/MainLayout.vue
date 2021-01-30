<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <router-view />
    </q-page-container>

    <div class="footer">
      <p class="footer_text">Приложение прогноза погоды, автор — Иванов Иван</p>
    </div>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",

  data() {
    return {};
  }
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}
.footer {
  height: 261px;
  width: 100%;
  background: #c4c4c4;
  &_text {
    padding-top: 160px;
    text-align: center;
    font-family: Roboto;
    font-size: 12px;
    font-style: normal;
    font-weight: 300;
    line-height: 14px;
    letter-spacing: 0em;
  }
}
</style>
