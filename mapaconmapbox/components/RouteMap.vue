<template>

</template>
<script>
import {poligonos} from "../static/map";
import {drawRoute} from "../scripts/dom";

export default {
  props: ['origin', 'destination'],
  inject: ['mapaRendered'],
  data() {
    return {
      routeObject: {
        stroke: String,
        strokeWidth: Number,
        strokeOpacity: Number,
        name: String,
        type: String,
        coordinates: Array,
      },
      routes: Object,
    }
  },

  mounted() {
    this.routes = poligonos
    for (var item of this.routes['features']) {

      if (item['geometry']['type'] == 'LineString') {
        this.routeObject.stroke = item['properties']['stroke'];
        this.routeObject.strokeWidth = item['properties']['stroke-width']
        this.routeObject.name = item['properties']['name']
        this.routeObject.type = item['properties']['type']
        this.routeObject.coordinates = item['geometry']['coordinates']
        drawRoute(this.mapaRendered(), this.routeObject)
      }
    }

  },
}
</script>
