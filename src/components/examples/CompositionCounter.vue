<template>
  <div class="composition-counter">
    <h3>⚙️ Composition API Компонент</h3>
    
    <div class="counter-display">
      <p>Счетчик: <span class="count">{{ count }}</span></p>
      <p>Удвоенный: <span class="count">{{ doubledCount }}</span></p>
      <p>Четный/нечетный: <span class="parity">{{ parity }}</span></p>
    </div>

    <div class="controls">
      <button @click="increment">+1</button>
      <button @click="decrement">-1</button>
      <button @click="reset">Сброс</button>
      <button @click="addRandom">Случайное число</button>
    </div>

    <div class="user-section">
      <h4>Информация о пользователе:</h4>
      <input v-model="userName" placeholder="Ваше имя" />
      <p v-if="userName">Привет, {{ userName }}!</p>
      <p>Имя введено: {{ hasUserName ? 'Да' : 'Нет' }}</p>
    </div>

    <div class="history-section">
      <h4>История действий:</h4>
      <ul>
        <li v-for="action in actionHistory" :key="action.id">
          {{ action.timestamp }}: {{ action.description }}
        </li>
      </ul>
      <button @click="clearHistory">Очистить историю</button>
    </div>

    <div class="lifecycle-info">
      <h4>Lifecycle события:</h4>
      <p>Компонент создан: {{ createdAt }}</p>
      <p>Компонент примонтирован: {{ mountedAt }}</p>
      <p v-if="updatedAt">Последнее обновление: {{ updatedAt }}</p>
    </div>

    <div class="composables-demo">
      <h4>🔧 Использование Composables:</h4>
      <p>Время работы компонента: {{ elapsedTime }}с</p>
      <p>Размер окна: {{ windowWidth }}x{{ windowHeight }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted, onUpdated, onBeforeUnmount } from 'vue'

// Composables (переиспользуемая логика)
function useTimer() {
  const elapsedTime = ref(0)
  let interval: NodeJS.Timeout

  onMounted(() => {
    interval = setInterval(() => {
      elapsedTime.value++
    }, 1000)
  })

  onBeforeUnmount(() => {
    clearInterval(interval)
  })

  return { elapsedTime }
}

function useWindowSize() {
  const windowWidth = ref(window.innerWidth)
  const windowHeight = ref(window.innerHeight)

  const updateSize = () => {
    windowWidth.value = window.innerWidth
    windowHeight.value = window.innerHeight
  }

  onMounted(() => {
    window.addEventListener('resize', updateSize)
  })

  onBeforeUnmount(() => {
    window.removeEventListener('resize', updateSize)
  })

  return { windowWidth, windowHeight }
}

// Основные данные
const count = ref(0)
const userName = ref('')
const actionHistory = ref<Array<{id: number, timestamp: string, description: string}>>([])
const createdAt = ref('')
const mountedAt = ref('')
const updatedAt = ref('')

// Composables
const { elapsedTime } = useTimer()
const { windowWidth, windowHeight } = useWindowSize()

// Вычисляемые свойства
const doubledCount = computed(() => count.value * 2)

const parity = computed(() => {
  return count.value % 2 === 0 ? 'четный' : 'нечетный'
})

const hasUserName = computed(() => userName.value.length > 0)

// Методы
const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}

const reset = () => {
  count.value = 0
  addAction('Счетчик сброшен')
}

const addRandom = () => {
  const randomNum = Math.floor(Math.random() * 10) + 1
  count.value += randomNum
  addAction(`Добавлено случайное число: ${randomNum}`)
}

const addAction = (description: string) => {
  actionHistory.value.push({
    id: Date.now(),
    timestamp: new Date().toLocaleTimeString(),
    description
  })
}

const clearHistory = () => {
  actionHistory.value = []
  addAction('История очищена')
}

// Watchers
watch(count, (newValue, oldValue) => {
  addAction(`Счетчик изменен с ${oldValue} на ${newValue}`)
  updatedAt.value = new Date().toLocaleTimeString()
})

watch(userName, (newValue) => {
  if (newValue) {
    addAction(`Имя изменено на: ${newValue}`)
  }
})

// Lifecycle hooks
onMounted(() => {
  createdAt.value = new Date().toLocaleTimeString()
  mountedAt.value = new Date().toLocaleTimeString()
  addAction('Компонент примонтирован')
  console.log('Composition API: onMounted()')
})

onUpdated(() => {
  console.log('Composition API: onUpdated()')
})

onBeforeUnmount(() => {
  console.log('Composition API: onBeforeUnmount()')
})

// Инициализация
addAction('Компонент создан')
</script>

<style scoped>
.composition-counter {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 2px solid #3498db;
  border-radius: 10px;
  background: linear-gradient(135deg, #d1ecf1 0%, #bee5eb 100%);
}

h3 {
  color: #2980b9;
  text-align: center;
  margin-bottom: 20px;
}

h4 {
  color: #3498db;
  margin: 15px 0 10px 0;
}

.counter-display {
  text-align: center;
  margin: 20px 0;
  padding: 15px;
  background: rgba(255,255,255,0.8);
  border-radius: 8px;
}

.count {
  font-size: 1.5em;
  font-weight: bold;
  color: #2980b9;
}

.parity {
  font-weight: bold;
  color: #27ae60;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
  flex-wrap: wrap;
}

.user-section, .history-section, .lifecycle-info, .composables-demo {
  margin: 20px 0;
  padding: 15px;
  background: rgba(255,255,255,0.7);
  border-radius: 8px;
  border-left: 4px solid #3498db;
}

ul {
  max-height: 150px;
  overflow-y: auto;
  list-style-type: none;
  padding: 0;
  margin: 10px 0;
}

li {
  padding: 5px;
  margin: 3px 0;
  background: rgba(255,255,255,0.5);
  border-radius: 4px;
  font-size: 0.9em;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: bold;
}

.controls button {
  background: #3498db;
  color: white;
}

.controls button:hover {
  background: #2980b9;
  transform: translateY(-1px);
}

input {
  padding: 8px;
  border: 2px solid #3498db;
  border-radius: 5px;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #2980b9;
}
</style>