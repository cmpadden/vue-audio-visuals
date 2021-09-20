<template>
  <div id="app">
    <Waveform :canvasHeight='250'/>
    <FrequencyBarGraph :canvasHeight='250'/>
    <Spectrogram :canvasHeight='250'/>
  </div>
</template>

<script>
import Waveform from './components/Waveform.vue'
import FrequencyBarGraph from './components/FrequencyBarGraph.vue'
import Spectrogram from './components/Spectrogram.vue'

export default {
  name: 'App',
  components: {
    Waveform,
    FrequencyBarGraph,
    Spectrogram
  }
}
</script>
