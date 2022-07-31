<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
	return b.createdAt - a.createdAt
}))


watch(name, (newValue) => {
	localStorage.setItem('name', newValue)
})

watch(todos, newValue => {
	localStorage.setItem('todos', JSON.stringify(newValue))
}, { deep: true })

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []

})

const addTodo = () => {
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}
	todos.value.push({
		content: input_content.value,
		category: input_category.value,
		done: false,
		createdAt: new Date().getTime()
	})

	input_content.value = ''
	input_category.value = null
}

const removeTodo = todo => {
	todos.value = todos.value.filter(t => t !== todo)
}



</script>

<template>

	<main class="app">
		<section class="greetings">
			<h2 class="title">
				Привет <input type="text" placeholder="ваше имя" v-model="name" />
			</h2>
		</section>

		<section class="create-todo">
			<h3>Создай свой туду или чек лист</h3>
			<form @submit.prevent="addTodo">
				<h4>Что нужно сделать ? </h4>
				<input type="text" placeholder="выучить Вью" v-model="input_content">
				<h4>Выбери категорию </h4>
				<div class="options">
					<label>
						<input type="radio" name="category" id="category1" value="business" v-model="input_category">
						<span class="business bubble"> </span>
						<div>Чеклист</div>
					</label>
					<label>
						<input type="radio" name="category" id="category1" value="personal" v-model="input_category">
						<span class="personal bubble"> </span>
						<div>Задание</div>
					</label>
				</div>
				<input type="submit" value="Добавить">
			</form>
		</section>


		<section class="todo-list">
			<h3>Список</h3>
			<div class="list">
				<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
					<label for="">
						<input type="checkbox" v-model="todo.done">
						<span :class="`bubble ${todo.category.value}`"></span>
					</label>

					<div class="todo-content">
						<input type="text" v-model="todo.content">
					</div>

					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Удалить</button>
					</div>
				</div>
			</div>

		</section>
	</main>

</template>