<template>
  <div id="app">
    <googlemaps-map :center.sync="center"
                    :zoom.sync="zoom"
                    :options="mapOptions"
                    @idle="onIdle"
                    @click="onMapClick">

      <!-- User Position -->
      <googlemaps-user-position @update:position="setUserPosition" />

      <googlemaps-marker v-for="marker of markers"
                         :key="marker._id"
                         :label="{
      color: marker === currentmarker ? 'white' : 'black',
      fontFamily: 'Material Icons',
      fontSize: '20px',
      text: 'star_rate',
    }"
                         :position="marker.position"
                         @click="selectMarker(marker._id)" />
    </googlemaps-map>
  </div>
</template>

<script>
  import { MapElement } from 'vue-googlemaps'

  const boundProps = [
    'animation',
    'clickable',
    'cursor',
    'draggable',
    // ...
  ]

  // Events from Google Maps emitted as Vue events
  const redirectedEvents = [
    'click',
    'rightclick',
    'dblclick',
    'drag',
    // ...
  ]

  export default {
    mixins: [
      // You need to use this mixin
      MapElement,
    ],

    // When Google Maps is ready
    googleMapsReady () {
      const options = Object.assign({}, this.$props)
      options.map = this.$_map

      // Create Google Maps objects
      this.$_marker = new window.google.maps.Marker(options)
      // Bind the Vue props
      this.bindProps(this.$_marker, boundProps)
      // Emit the events from Google Maps
      this.redirectEvents(this.$_marker, redirectedEvents)
    },

    beforeDestroy () {
      // Teardown
      if (this.$_marker) {
        this.$_marker.setMap(null)
      }
    },
  }
</script>

<style>
  .amap-wrapper {
    width: 500px;
    height: 500px;
  }
</style>
