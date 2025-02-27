<template> 
  <div class="progress-bar">
    <svg
      version="1.1"
      baseProfile="full"
      width="300"
      height="300"
      viewBox="0 0 200 200"
      xmlns="http://www.w3.org/2000/svg"
      class="pie">
      
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
        fill="red"
        stroke="black"
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
      <text 
        :x="cx" 
        :y="cy" 
        text-anchor="middle" 
        alignment-baseline="middle" 
        font-size="20"
        fill="black">
        {{ percent }}%
      </text>
    </svg>
    
    <!-- Кнопки управления -->
    <div class="controls">
      <button @click="decreasePercent">-10%</button>
      <button @click="increasePercent">+10%</button>
    </div>
  </div>
</template>

<script>
export default {
    name: 'ProgressBar',
    data() {
        return {
            cx: 100, // Центр по X
            cy: 100, // Центр по Y
            radius: 90, // Внешний радиус
            innerRadius: 80, // Внутренний радиус (для кольца)
            percent: 0, // Процент для дуги
        };
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

            return `M ${cx},${cy} 
                    L ${startX},${startY} 
                    A ${radius},${radius} 0 ${largeArcFlag},1 ${x},${y} 
                    Z`;
        }
    },
    methods: {
        // Увеличить проценты
        increasePercent() {
            if (this.percent < 100) {
                this.percent += 10;
            }
        },
        // Уменьшить проценты
        decreasePercent() {
            if (this.percent > 0) {
                this.percent -= 10;
            }
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