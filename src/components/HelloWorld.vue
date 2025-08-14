<template>
  <div class="api-comparison">
    <h1>Options API vs Composition API</h1>
    
    <!-- Переключатель между примерами -->
    <div class="switcher">
      <button 
        @click="currentExample = 'options'"
        :class="{ active: currentExample === 'options' }"
      >
        Options API (Vue 2 стиль)
      </button>
      <button 
        @click="currentExample = 'composition'"
        :class="{ active: currentExample === 'composition' }"
      >
        Composition API (Vue 3 стиль)
      </button>
    </div>

    <!-- Options API компонент -->
    <div v-if="currentExample === 'options'" class="example-container">
      <h2>📦 Options API - объектный подход</h2>
      <OptionsCounter />
    </div>

    <!-- Composition API компонент -->
    <div v-if="currentExample === 'composition'" class="example-container">
      <h2>🔧 Composition API - функциональный подход</h2>
      <CompositionCounter />
    </div>

    <!-- Сравнительная таблица -->
    <div class="comparison-table">
      <h2>📊 Сравнение подходов</h2>
      <table>
        <thead>
          <tr>
            <th>Аспект</th>
            <th>Options API</th>
            <th>Composition API</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><strong>Синтаксис</strong></td>
            <td>Объектный, разделен по секциям</td>
            <td>Функциональный, все в setup()</td>
          </tr>
          <tr>
            <td><strong>Логика</strong></td>
            <td>Разбросана по data, methods, computed</td>
            <td>Группируется по функциональности</td>
          </tr>
          <tr>
            <td><strong>TypeScript</strong></td>
            <td>Сложнее типизировать</td>
            <td>Отличная поддержка типов</td>
          </tr>
          <tr>
            <td><strong>Переиспользование</strong></td>
            <td>Mixins (устаревший подход)</td>
            <td>Composables (современно)</td>
          </tr>
          <tr>
            <td><strong>Сложность</strong></td>
            <td>Проще для новичков</td>
            <td>Мощнее для сложных проектов</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Практические примеры -->
    <div class="practical-examples">
      <h2>🛠 Практические примеры</h2>
      
      <div class="side-by-side">
        <div class="code-block">
          <h3>Options API</h3>
          <pre><code>export default {
  data() {
    return {
      count: 0,
      users: []
    }
  },
  computed: {
    doubleCount() {
      return this.count * 2
    }
  },
  methods: {
    increment() {
      this.count++
    },
    async fetchUsers() {
      const response = await api.getUsers()
      this.users = response.data
    }
  },
  mounted() {
    this.fetchUsers()
  }
}</code></pre>
        </div>

        <div class="code-block">
          <h3>Composition API</h3>
          <pre><code>&lt;script setup lang="ts"&gt;
import { ref, computed, onMounted } from 'vue'

const count = ref(0)
const users = ref([])

const doubleCount = computed(() => count.value * 2)

const increment = () => {
  count.value++
}

const fetchUsers = async () => {
  const response = await api.getUsers()
  users.value = response.data
}

onMounted(() => {
  fetchUsers()
})
&lt;/script&gt;</code></pre>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import OptionsCounter from './examples/OptionsCounter.vue'
import CompositionCounter from './examples/CompositionCounter.vue'

type ExampleType = 'options' | 'composition'

const currentExample = ref<ExampleType>('options')
</script>

<style scoped>
.api-comparison {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}

.switcher {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
}

.switcher button {
  padding: 12px 24px;
  border: 2px solid #3498db;
  background: white;
  color: #3498db;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: bold;
}

.switcher button.active,
.switcher button:hover {
  background: #3498db;
  color: white;
}

.example-container {
  margin: 30px 0;
  padding: 20px;
  border: 2px solid #34495e;
  border-radius: 10px;
  background: #f8f9fa;
}

.comparison-table {
  margin: 30px 0;
}

table {
  width: 100%;
  border-collapse: collapse;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background: #34495e;
  color: white;
  font-weight: bold;
}

tr:nth-child(even) {
  background: #f8f9fa;
}

tr:hover {
  background: #e3f2fd;
}

.practical-examples {
  margin: 30px 0;
}

.side-by-side {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin: 20px 0;
}

.code-block {
  background: #2c3e50;
  border-radius: 8px;
  overflow: hidden;
}

.code-block h3 {
  background: #34495e;
  color: white;
  padding: 15px;
  margin: 0;
  text-align: center;
}

.code-block pre {
  margin: 0;
  padding: 20px;
  color: #ecf0f1;
  font-size: 13px;
  line-height: 1.4;
  overflow-x: auto;
}

.code-block code {
  font-family: 'Fira Code', 'Courier New', monospace;
}

@media (max-width: 768px) {
  .side-by-side {
    grid-template-columns: 1fr;
  }
  
  .switcher {
    flex-direction: column;
    align-items: center;
  }
  
  .switcher button {
    width: 200px;
  }
}
</style>