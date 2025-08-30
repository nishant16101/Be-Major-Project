# OpenFreeMap + MapLibre GL JS Examples

Basic examples using [OpenFreeMap.org](https://openfreemap.org/) tiles with [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/docs/) libraries.

* Each example:
  1. Loads the base MapLibre GL JS map
    * style url set to `https://tiles.openfreemap.org/styles/liberty`
    * option to set style url to `/styles/dark.json`
      * hosted in the public folder of each app
      * dark style adapted from maptiler.com basic-v2-dark style
  2. Gets the users location via [http://ip-api.com/json/](http://ip-api.com/json/)
  3. Uses [`map.flyTo`](https://maplibre.org/maplibre-gl-js/docs/API/classes/Map/#flyto) to animate / zoom to the users location
  4. Displays a Popup on the map at the users location

## Examples

* [Vanilla JS / HTML](./vanilla-example/)
  * built with core MapLibre GL JS loaded from unpkg cdn
* [React Example](./react-example/)
  * built with [@vis.gl/react-maplibre](https://visgl.github.io/react-maplibre/)
* [Vue Example](./vue-example/)
  * built with [@indoorequal/vue-maplibre-gl](https://indoorequal.github.io/vue-maplibre-gl/)
* [Svelte Example](./svelte-example/)
  * built with [svelte-maplibre](https://github.com/dimfeld/svelte-maplibre)
* [Vanilla JS / HTML + LeafletJS](./vanilla-leaflet-example/)
  * same as vanilla-example but uses [leaflet](https://leafletjs.com/) instead