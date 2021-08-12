<template>
  <div class="yandex">
    <div id="map" style="width: 100%; height: 500px; padding: 2rem 0 "></div>
  </div>
</template>

<script>

export default {
  name: "Yandex",
  data: () => ({
    map: null,
    controls: [],
  }),
  created() {

  },
  computed: {},
  methods: {

    initializeYandexMap() {
      // eslint-disable-next-line no-undef
      return new Promise(r => ymaps.ready(() => {
        // eslint-disable-next-line no-undef
        this.map = new ymaps.Map("map", {
          center: [55.76, 37.64],
          zoom: 20,
          controls: ['fullscreenControl','rulerControl','smallMapDefaultSet','geolocationControl'],
          searchControlProvider: 'yandex#search',
        }, {
          restrictMapArea: [
            [55.76, 37.64],
            [55.96, 37.84]
          ]
        });
        let searchControl = new ymaps.control.SearchControl({
          options: {
            provider: 'yandex#search'
          }
        });


        this.map.controls.add(searchControl);
        searchControl.search('АЗС');
        r();
      }));

    },
  },
  mounted() {
    let scriptYandexMap = document.createElement('script');
    scriptYandexMap.setAttribute('src', 'https://api-maps.yandex.ru/2.1/?apikey=0c94976f-5b19-4562-adab-814369f61605&lang=ru_RU');
    document.head.appendChild(scriptYandexMap);

    // Инициализировать яндекс карту
    scriptYandexMap.addEventListener('load', () => Promise.all([
      this.initializeYandexMap(),
    ]));

  }
}
</script>

<style scoped>

</style>