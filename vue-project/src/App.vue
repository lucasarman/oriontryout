<template>
  <div id="app">
    <AppSidebar
      :shapes="shapes"
      @loadShape="(index) => $refs.mapComponent.loadShape(index)"
      @removeShape="removeShape"
      @toggleDrawing="toggleDrawing"
      @clearShapes="clearShapes"
    />
    <MapComponent
      ref="mapComponent"
      :shapes="shapes"
      @addShape="addShape"
      :drawingMode="drawingMode"
    />
  </div>
</template>

<script>
import axios from 'axios'
import AppSidebar from './components/AppSidebar.vue'
import MapComponent from './components/MapComponent.vue'

export default {
  components: { AppSidebar, MapComponent },
  data() {
    return {
      shapes: [], 
      drawingMode: null, 
    }
  },
  async created() {
    try {
      const response = await axios.get('http://localhost:8080/')
      this.shapes = response.data 
    } catch (error) {
      console.error('Erro ao carregar dados:', error.message)
    }
  },
  methods: {
    addShape(shape) {
      this.shapes.push(shape) 
    },
    removeShape(index) {
      const shape = this.shapes[index]
      if (shape.overlay) {
        shape.overlay.setMap(null) 
        google.maps.event.clearInstanceListeners(shape.overlay) 
      }
      this.shapes.splice(index, 1) 
    },
    clearShapes() {
      this.shapes.forEach((shape) => {
        if (shape.overlay) {
          console.log(shape.overlay)
          shape.overlay.setMap(null) 
          google.maps.event.clearInstanceListeners(shape.overlay) 
        }
      })
      this.shapes = [] 
    },
    toggleDrawing(type) {
      this.drawingMode = type 
    },
  },
}
</script>

<style>
#app {
  display: flex;
  width: 100%;
  height: 100vh;
}
</style>
