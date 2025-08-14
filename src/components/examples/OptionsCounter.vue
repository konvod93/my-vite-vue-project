<template>
  <div class="options-counter">
    <h3>🏗 Options API Компонент</h3>
    
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
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

interface Action {
  id: number
  timestamp: string
  description: string
}

export default defineComponent({
  name: 'OptionsCounter',
  
  // 📦 ДАННЫЕ - реактивные свойства
  data() {
    return {
      count: 0,
      userName: '',
      actionHistory: [] as Action[],
      createdAt: '',
      mountedAt: '',
      updatedAt: ''
    }
  },
  
  // 🧮 ВЫЧИСЛЯЕМЫЕ СВОЙСТВА
  computed: {
    doubledCount(): number {
      return this.count * 2
    },
    
    parity(): string {
      return this.count % 2 === 0 ? 'четный' : 'нечетный'
    },
    
    hasUserName(): boolean {
      return this.userName.length > 0
    }
  },
  
  // 👀 НАБЛЮДАТЕЛИ
  watch: {
    count(newValue: number, oldValue: number) {
      this.addAction(`Счетчик изменен с ${oldValue} на ${newValue}`)
      this.updatedAt = new Date().toLocaleTimeString()
    },
    
    userName(newValue: string) {
      if (newValue) {
        this.addAction(`Имя изменено на: ${newValue}`)
      }
    }
  },
  
  // 🎯 МЕТОДЫ
  methods: {
    increment() {
      this.count++
    },
    
    decrement() {
      this.count--
    },
    
    reset() {
      this.count = 0
      this.addAction('Счетчик сброшен')
    },
    
    addRandom() {
      const randomNum = Math.floor(Math.random() * 10) + 1
      this.count += randomNum
      this.addAction(`Добавлено случайное число: ${randomNum}`)
    },
    
    addAction(description: string) {
      this.actionHistory.push({
        id: Date.now(),
        timestamp: new Date().toLocaleTimeString(),
        description
      })
    },
    
    clearHistory() {
      this.actionHistory = []
      this.addAction('История очищена')
    }
  },
  
  // 🔄 ЖИЗНЕННЫЙ ЦИКЛ
  created() {
    this.createdAt = new Date().toLocaleTimeString()
    this.addAction('Компонент создан')
    console.log('Options API: created()')
  },
  
  mounted() {
    this.mountedAt = new Date().toLocaleTimeString()
    this.addAction('Компонент примонтирован')
    console.log('Options API: mounted()')
  },
  
  updated() {
    console.log('Options API: updated()')
  },
  
  beforeUnmount() {
    console.log('Options API: beforeUnmount()')
  }
})
</script>

<style scoped>
.options-counter {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 2px solid #e74c3c;
  border-radius: 10px;
  background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
}

h3 {
  color: #c0392b;
  text-align: center;
  margin-bottom: 20px;
}

h4 {
  color: #e74c3c;
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
  color: #c0392b;
}

.parity {
  font-weight: bold;
  color: #e67e22;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
  flex-wrap: wrap;
}

.user-section, .history-section, .lifecycle-info {
  margin: 20px 0;
  padding: 15px;
  background: rgba(255,255,255,0.7);
  border-radius: 8px;
  border-left: 4px solid #e74c3c;
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
  background: #e74c3c;
  color: white;
}

.controls button:hover {
  background: #c0392b;
  transform: translateY(-1px);
}

input {
  padding: 8px;
  border: 2px solid #e74c3c;
  border-radius: 5px;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #c0392b;
}
</style>