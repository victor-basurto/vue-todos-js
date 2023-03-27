<script setup>
import { computed, ref, watch } from 'vue'
import { uid } from 'uid'
import { Icon } from '@iconify/vue'
import TodoCreator from '../components/TodoCreator.vue'
import TodoItem from '../components/TodoItem.vue'

const todoList = ref([])

watch(todoList, (newVal, oldValue) => {
	setTodoListToLocalStorage()
}, {
	deep: true
})
const todoCompleted = computed(() => todoList.value.every((todo) => todo.isCompleted))
const fetchTodoList = () => {
	const savedTodoList = JSON.parse(localStorage.getItem('todoList'))
	if (savedTodoList) {
		todoList.value = savedTodoList
	}
}
fetchTodoList()

const createTodo = (todo) => {
	todoList.value.push({
		id: uid(),
		...todo,
		isCompleted: null,
		isEditing: null
	})
}
const setTodoListToLocalStorage = () => localStorage.setItem('todoList', JSON.stringify(todoList.value))
const toggleTodoComplete = todoPos => todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
const toggleEditTodo = todoPos => todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing
const updateTodo = (todoVal, todoPos) => todoList.value[todoPos].todo = todoVal
const deleteTodo = todoId => todoList.value = todoList.value.filter(todo => todo.id !== todoId)
</script>

<template>
	<main>
		<h1>Create Todo</h1>
		<TodoCreator @create-todo="createTodo"/>
		<ul class="todo-list" v-if="todoList.length > 0">
			<TodoItem
				v-for="(item, index) in todoList"
				:todo="item"
				:index="index"
				@toggle-complete="toggleTodoComplete"
				@edit-todo="toggleEditTodo"
				@update-todo="updateTodo"
				@delete-todo="deleteTodo"
			/>
		</ul>
		<p class="todos-msg" v-else>
			<Icon icon="mdi:emoticon-sad-outline" width="22" height="22" />
			<span>You have no TODO's completed! Add one!</span>
		</p>
		<p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
			<Icon icon="lucide:party-popper" color="#41af7f" width="22" height="22" />
			<span>You have completed all of your TODO's!</span>
		</p>
	</main>
</template>
<style lang="scss" scoped>
main {
	display: flex;
	flex-direction: column;
	max-width: 500px;
	width: 100%;
	margin: 0 auto;
	padding: 40px 16px;
	h1 {
		margin-bottom: 16px;
		text-align: center;
	}
	.todo-list {
		display: flex;
		flex-direction: column;
		list-style: none;
		margin-top: 24px;
		gap: 20px;
	}
	.todos-msg {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 8px;
		margin-top: 24px;
	}
}
</style>
