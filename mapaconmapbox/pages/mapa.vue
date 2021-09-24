<template>
  <div class="columns">
    <general-map
      style="height: 100vh; width: 50%"
      :center="[-38.748933,-72.617708]"
      :zoom="15"
      @mb-created="(mapReference) => map = mapReference"
    >
      <div v-for="(item, i) in poligonos['features']" :key="i">
        <polygon-map v-if="item['properties']['type'] != 'route'" :visibility="verifyType(item['properties']['floor'])" :geojson="item"></polygon-map>
      </div>
      <RouteMap></RouteMap>

    </general-map>

    <div class="column" style="background-color: #F8F7FD">
      <div class="p-3 m-6 has-background-white">
        <section>
<!--          <b-field v-for="item in filtros" :key="item">-->
<!--            <b-checkbox :native-value="item" v-model="filtersChosen">{{ item }}</b-checkbox>-->
<!--          </b-field>-->
          <p>Edificio DCI</p>
          <b-field>
            <b-radio v-model="filtersChosen" :native-value="1">Primer Piso</b-radio>
            <b-radio v-model="filtersChosen" :native-value="2">Segundo Piso</b-radio>
          </b-field>
        </section>
      </div>
    </div>


  </div>

</template>

<script>
import PolygonMap from "../components/PolygonMap";
import GeneralMap from "../components/GeneralMap";
import {poligonos} from "../static/map";
import RouteMap from "../components/RouteMap";


export default {
  name: "mapa",
  components: {
    PolygonMap,
    GeneralMap,
    RouteMap
  },
  data() {
    return {
      filtros: [],
      filtersChosen: 1,
      poligonos: {},
      map: {},
      poligonsOnTheFirstFloor: []
    }
  },

  mounted() {
    this.poligonos = poligonos
    this.filtros = this.getTypes()
    this.getFirstFloor()
    // this.filtersChosen = this.filtros
  },

  methods: {
    getTypes() {
      var types = poligonos['features'].map(function (item) {
        return item['properties']['type']
      })
      const dataArr = new Set(types)
      return [...dataArr]
    },

    getFirstFloor() {
      var polygonsOnTheFirstFloor = this.poligonos['features'].filter((item) => item['properties']['floor'] == 1)
      this.poligonsOnTheFirstFloor = polygonsOnTheFirstFloor
    },

  },

  computed: {
    verifyType() {
      return (floor) => {
        if (floor) {
          return this.filtersChosen == floor
        } else {
          return true
        }
      }
    }
  },
}
</script>

<style>

</style>
