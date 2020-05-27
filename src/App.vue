<template>
  <div id="app">
    <div id="map" ref="map">
      <map-marker
      :lat="-27.344"
      :lng="133.036">
      </map-marker>
      <map-marker
      :lat="-26.344"
      :lng="132.036">
      </map-marker>
      <map-marker
      :lat="-25.344"
      :lng="131.036">
      </map-marker>
    </div>
    <button
    class="button"
    @click="buttonClick"></button>
  </div>
</template>

<script>
import MapMarker from "./components/MapMarker.vue"
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    MapMarker
  },
  data: () => ({
    map:null,
    center: {
      lat: null,
      lng: null
    }
  }),
  mounted() {
    if (navigator.geolocation) {
      // callback関数内でthis使えないため
      let vm = this
      navigator.geolocation.getCurrentPosition(
        function(position){
          let latlng = new window.google.maps.LatLng(
            position.coords.latitude,
            position.coords.longitude
          );

          vm.map = new window.google.maps.Map(vm.$refs["map"], {
            center: latlng,
            zoom: 4
          })
          new window.google.maps.Marker({
            position: latlng,
            map: vm.map
          })
        }
      )
    }
  },
  methods: {
    getMap(callback) {
      let vm = this
      function checkForMap() {
        if(vm.map) callback(vm.map)
        else setTimeout(checkForMap, 200)
      }
      checkForMap()
    },
    buttonClick() {
      this.center.lat = -25.344
      this.center.lng = 131.036
      console.log(this.center.lat)
      console.log(this.center.lng)

      // window.navigator.geolocation.getCurrentPosition(function(position) {
      //   this.center.lat = position.coords.latitude
      //   this.center.lng = position.coords.longitude
      //   console.log(this.center.lat)
      //   console.log(this.center.lng)
      // })
    }
  }
};
</script>

<style>
#map {
  height: 600px;
  background: gray;
}
.button {
  height: 40px;
  width: 40px;
  background-color: red;
}
</style>
