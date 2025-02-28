<template>
  <div>
    <ProgressBar 
      :width="progressWidth" 
      :height="progressHeight" 
      :cx="progressCX" 
      :cy="progressCY" 
      :radius="progressRadius" 
      :innerRadius="progressInnerRadius" 
      :percent="percent" 
      :warning="warning" 
      :error="error" 
      :speed="speed"
    />
    
    <div>
      <label for="width">Ширина: </label>
      <input type="number" id="width" v-model="progressWidth" :min="100" :max="500" step="10" />
    </div>

    <div>
      <label for="height">Высота: </label>
      <input type="number" id="height" v-model="progressHeight" :min="100" :max="500" step="10" />
    </div>

    <div>
      <label for="cx">Центр X: </label>
      <input type="number" id="cx" v-model="progressCX" :min="0" :max="progressWidth" step="1" />
    </div>

    <div>
      <label for="cy">Центр Y: </label>
      <input type="number" id="cy" v-model="progressCY" :min="0" :max="progressHeight" step="1" />
    </div>

    <div>
      <label for="radius">Радиус: </label>
      <input type="number" id="radius" v-model="progressRadius" :min="20" :max="Math.min(progressWidth, progressHeight) / 2" step="10" />
    </div>

    <div>
      <label for="innerRadius">Внутренний радиус: </label>
      <input type="number" id="innerRadius" v-model="progressInnerRadius" :min="10" :max="Math.min(progressWidth, progressHeight) / 2" step="10" />
    </div>

    <div>
      <label for="percent">Процент: </label>
      <input type="number" id="percent" v-model="percent" min="0" max="100" step="1" />
    </div>

    <div>
      <label for="warning">Предупреждение: </label>
      <input type="checkbox" id="warning" v-model="warning" />
    </div>

    <div>
      <label for="error">Ошибка: </label>
      <input type="checkbox" id="error" v-model="error" />
    </div>
  </div>
</template>

<script>
import ProgressBar from './ProgressBar.vue';

export default {
  name: 'PanelProgressbar',
  components: {
    ProgressBar
  },
  data() {
    return {
      progressWidth: 300,
      progressHeight: 300,
      progressCX: 100,
      progressCY: 100,
      progressRadius: 90,
      progressInnerRadius: 80,
      percent: 0,
      warning: false,
      error: false,
      speed: 50
    };
  }
};
</script>

<style scoped>
div {
  margin-bottom: 10px;
}

label {
  margin-right: 10px;
}

input {
  padding: 5px;
  margin-left: 10px;
}
</style>
