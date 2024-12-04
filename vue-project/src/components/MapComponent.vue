<template>
  <div>
    <div id="map" ref="map" style="height: 100vh; width: 100vh"></div>
  </div>
</template>

<script>
import { ref, watch, onMounted } from 'vue'
import { Loader } from '@googlemaps/js-api-loader'

export default {
  props: {
    shapes: {
      type: Array,
      required: true,
    },
    drawingMode: {
      type: String,
      default: null,
    },
  },
  setup(props, { emit }) {
    const map = ref(null)
    const drawingManager = ref(null)

    onMounted(async () => {
      const loader = new Loader({
        apiKey: 'AIzaSyAKVzU2mCcpNv3mqFdLuhKK7ydut_PgpVc',
        version: 'weekly',
        libraries: ['drawing'],
      })

      const google = await loader.load()

      
      map.value = new google.maps.Map(document.getElementById('map'), {
        center: { lat: -14.235, lng: -51.9253 }, 
        zoom: 4,
      })


      
      drawingManager.value = new google.maps.drawing.DrawingManager({
        drawingControl: false,
        rectangleOptions: { editable: true, draggable: true },
        circleOptions: { editable: true, draggable: true },
        polygonOptions: { editable: true, draggable: true },
      })

      google.maps.event.addListener(drawingManager.value, 'overlaycomplete', (event) => {
        const shape = {
          id: Date.now(),
          type: event.type,
          overlay: event.overlay,
          bounds: event.overlay.getBounds?.() || null, // Retângulo
          center: event.overlay.getCenter?.() || null, // Círculo
          paths: event.overlay.getPath?.().getArray() || null, // Polígono
        }

        
        emit('addShape', shape)

        
        drawingManager.value.setDrawingMode(null)
      })

      drawingManager.value.setMap(map.value)
    })
    watch(
      () => props.drawingMode,
      (newMode) => {
        if (drawingManager.value) {
          drawingManager.value.setDrawingMode(newMode)
        }
      },
    )

    return {
      map,
    }
  },
}
</script>

<style scoped>
#map {
  margin-bottom: 20px;
}
</style>
