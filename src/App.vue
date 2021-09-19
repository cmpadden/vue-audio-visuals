<template>
  <div id="app">
    <Waveform canvasHeight='250'/>
    <FrequencyBarGraph canvasHeight='250'/>
  </div>
</template>

<script>
// import Spectrogram from './components/Spectrogram.vue'
import Waveform from './components/Waveform.vue'
import FrequencyBarGraph from './components/FrequencyBarGraph.vue'

export default {
  name: 'App',
  components: {
    Waveform,
    FrequencyBarGraph
  }
}
</script>
