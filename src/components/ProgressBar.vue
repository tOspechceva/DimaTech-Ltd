<template> 
  <div class="progress-bar">
    <svg
      version="1.1"
      baseProfile="full"
      :width="width"
      :height="height"
      viewBox="0 0 200 200"
      >
      
      <!-- Основной круг -->
      <circle 
        :cx="cx" 
        :cy="cy" 
        :r="radius" 
        class="circle_grey" 
        fill="lightgrey"
      />
      
      <!-- Сектор дуги -->
      <path 
        :d="pathD" 
        :fill="pathColor" 
        stroke="white"
        stroke-width="1"
      />
      
      <!-- Внутренний круг (для эффекта "кольца") -->
      <circle 
        :cx="cx" 
        :cy="cy" 
        :r="innerRadius" 
        fill="white" 
      />
      
      <!-- Текст в центре круга -->
          <!-- Текст в центре круга -->
    <text
      v-if="!warning && !error && percent < 100"
      :x="cx"
      :y="cy + 10"
      text-anchor="middle"
      font-size="24"
      fill="black"
    >
      {{ percent }}%
    </text>

    <!-- Галочка при 100% -->
    <text
      v-if="percent === 100"
      :x="cx"
      :y="cy + 10"
      text-anchor="middle"
      font-size="32"
      fill="green"
      font-weight="bold"
    >
      ✔
    </text>

        <!-- Восклицательный знак при warning -->
    <text
      v-if="warning"
      :x="cx"
      :y="cy + 10"
      text-anchor="middle"
      font-size="32"
      fill="orange"
      font-weight="bold"
    >
      !
    </text>

    <!-- Крест при error -->
    <text
      v-if="error"
      :x="cx"
      :y="cy + 10"
      text-anchor="middle"
      font-size="32"
      fill="red"
      font-weight="bold"
    >
      ✖
    </text>
    </svg>
    
    
  </div>
</template>

<script>
export default {
    name: 'ProgressBar',
    props: {
    width: {
      type: Number,
      default: 300,
    },
    height: {
      type: Number,
      default: 300,
    },
    cx: {
      type: Number,
      default: 100,
    },
    cy: {
      type: Number,
      default: 100,
    },
    radius: {
      type: Number,
      default: 90,
    },
    innerRadius: {
      type: Number,
      default: 80,
    },
    percent: {
      type: Number,
      default: 0,
    },
    warning: {
      type: Boolean,
      default: false,
    },
    error: {
      type: Boolean,
      default: false,
    },
    speed: {
      type: Number,
      default: 50,
    },
  },
    computed: {
      // Расчёт пути для дуги
      pathD() {
        const { cx, cy, radius, percent } = this;
        
        
        const angle = percent * 3.6; // 1% = 3.6°
        const radians = (angle - 90) * (Math.PI / 180); // Смещение на -90° для старта сверху
          
        // Начальная точка дуги (вверх от центра)
        const startX = cx;
        const startY = cy - radius;

        // Конечные координаты дуги
        const x = cx + radius * Math.cos(radians);
        const y = cy + radius * Math.sin(radians);

        // Определение дуги (большая или малая)
        const largeArcFlag = percent > 50 ? 1 : 0;

        // Если 0%, рисуем точку вверху (скрываем дугу)
        if (percent === 0) {
            return '';
        }
        // Если 100%, рисуем полный круг
        if (percent === 100) {
        return `M ${cx},${cy} 
                m 0,-${radius} 
                a ${radius},${radius} 0 1,0 0,${2 * radius} 
                a ${radius},${radius} 0 1,0 0,-${2 * radius} 
                Z`;
        }
        return `M ${cx},${cy} 
                L ${startX},${startY} 
                A ${radius},${radius} 0 ${largeArcFlag},1 ${x},${y} 
                Z`;
      },
        // Вычисление цвета сектора в зависимости от процента
      pathColor() {
        let hue;
        if (this.warning) {
          hue = 20;
        } else if (this.percent <= 25 || this.error) {
          hue = 8; 
        }  else if(this.percent > 25 && this.percent <= 75) {
          hue = 202; 
        }else{
          // Плавный переход от синего (240°) к зелёному (120°) на 100%
          hue = 202 - ((this.percent - 75) / 25) * 80; // от 240 до 120
        }
        return `hsl(${hue}, 90%, 50%)`; // Смена цвета
      }
  },
    methods: {
        // Увеличить проценты на 10
        increasePercent() {
          if (this.percent < 100) {
            const targetPercent = Math.min(this.percent + 10, 100); // Нужный процент, не больше 100
            this.animateToPercent(targetPercent);
          }
        },
        // Уменьшить проценты на 10
        decreasePercent() {
          if (this.percent > 0) {
            const targetPercent = Math.max(this.percent - 10, 0); // Нужный процент, не меньше 0
            this.animateToPercent(targetPercent);
          }
        },
        // Плавно анимировать изменение процента
        animateToPercent(targetPercent) {
          const step = 1; // Шаг изменения процента
          const interval = setInterval(() => {
            if (this.percent < targetPercent) {
              this.percent += step;
            } else if (this.percent > targetPercent) {
              this.percent -= step;
            } else {
              clearInterval(interval); // Остановить анимацию, если достигли цели
            }
          }, this.speed); // Интервал для обновления, можно настроить скорость
        }
    }
}
</script>

<style scoped>
.progress-bar {
    text-align: center;
    margin-top: 20px;
}

.controls {
    margin-top: 10px;
}

button {
    margin: 0 5px;
    padding: 5px 10px;
    font-size: 16px;
    cursor: pointer;
    border: 1px solid #ccc;
    border-radius: 4px;
    background-color: #f0f0f0;
    transition: background 0.3s;
}

button:hover {
    background-color: #e0e0e0;
}

</style>
