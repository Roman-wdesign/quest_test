<template>
  <div class="yandex">

    <h4 v-show="checkbox">Map</h4>
    <h4 v-show="!checkbox">List</h4>
    <div class="panel panel-default ">

      <div class="panel-body">
        <label class="switch">
          <input type="checkbox" @click="toggleCheckbox">
          <div class="slider round"></div>
        </label>
      </div>


      <div id="map" v-show="checkbox"

           style="width: 100%; height: 500px; padding: 2rem 0"
      ></div>
      <div class="yandex_map_results" v-show="!checkbox">
        <ul>
          <li
              v-for="value in groups"
              :key="value">
            address:{{ value }}
          </li>
        </ul>
        <h3>results</h3>
      </div>
    </div>

  </div>
</template>

<script>


export default {
  name: "Yandex",
  components: {},
  data: () => ({
    groups: [],
    checkbox: true,
  }),
  created() {

  },
  computed: {},
  methods: {
    getCoordData() {
      return new Promise(r => setTimeout(() => {
        this.groups = [

          [55.778922068976605,37.71569549999996, "Москва, улица Семёновский Вал, 4А"],
          [55.77851343420828, 37.74265999206541, "Москва, Вольная улица, вл39"],

        ];
        r();
      }, 1000));
    },
    setMarkers() {
      for (let i = 0; i < this.groups.length; i++) {
        // eslint-disable-next-line no-undef
        let placemark = new ymaps.Placemark(this.groups[i]);
        this.map.geoObjects.add(placemark);
      }
    },

    initializeYandexMap() {
      // eslint-disable-next-line no-undef
      return new Promise(r => ymaps.ready(() => {
        // eslint-disable-next-line no-undef
        this.map = new ymaps.Map("map", {
          center: [55.76, 37.64],
          zoom: 20,
          controls: ['fullscreenControl', 'rulerControl'],
          searchControlProvider: 'yandex#search',

        }, {
          restrictMapArea: [
            [55.76, 37.64],
            [55.96, 37.84]
          ]
        });

        // eslint-disable-next-line no-undef
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
    toggleCheckbox() {
      this.checkbox = !this.checkbox
      this.$emit('setCheckboxVal', this.checkbox)
    }
  },
  mounted() {
    let scriptYandexMap = document.createElement('script');
    scriptYandexMap.setAttribute('src', 'https://api-maps.yandex.ru/2.1/?apikey=0c94976f-5b19-4562-adab-814369f61605&lang=ru_RU');
    document.head.appendChild(scriptYandexMap);

    // Инициализировать яндекс карту
    scriptYandexMap.addEventListener('load', () => Promise.all([
      this.initializeYandexMap(),
      this.getCoordData(),
    ]).then(this.setMarkers));

  }
}
</script>

<style lang="scss" scoped>
</style>