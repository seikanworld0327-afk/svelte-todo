<script lang="ts">
  import type { Todo } from '../types/todo';
  import { onMount } from 'svelte';
  import TodoStats from '$lib/components/TodoStats.svelte';
  import ConfirmModal from '$lib/components/ConfirmModal.svelte';

  let todos = $state([] as Todo[]);
  let newTodo: string = $state('');
  let isInitialized: boolean = $state(false);
  let showClearConfirm: boolean = $state(false);

  let remainingCount = $derived(todos.filter((todo) => !todo.done).length);
  let completedCount = $derived(todos.filter((todo) => todo.done).length);

  onMount(() => {
    if (typeof window !== 'undefined') {
      try {
        const savedTodos = localStorage.getItem('todos');
        if (savedTodos) {
          todos = JSON.parse(savedTodos);
        }
      } catch (e) {
        console.error('Failed to load todos from localStorage:', e);
      } finally {
        isInitialized = true;
      }
    }
  });

  $effect(() => {
    if (isInitialized && typeof window !== 'undefined') {
      try {
        localStorage.setItem('todos', JSON.stringify(todos));
      } catch (e) {
        console.error('Failed to save todos to localStorage:', e);
      }
    }
  });

  function resetInput() {
    newTodo = '';
  }

  function addTodo() {
    const trimmed = newTodo.trim();
    if (trimmed) {
      todos.push({ text: trimmed, done: false });
      resetInput();
    }
  }

  function deleteTodo(index: number) {
    todos.splice(index, 1);
  }

  function requestClearCompleted() {
    showClearConfirm = true;
  }

  function confirmClearCompleted() {
    todos = todos.filter((todo) => !todo.done);
    showClearConfirm = false;
  }

  function cancelClearCompleted() {
    showClearConfirm = false;
  }
</script>

<main class="min-h-screen bg-gray-100 flex flex-col items-center py-12">
  <h1 class="text-4xl font-bold text-gray-800 mb-8">Todo App</h1>

  <div class="flex items-center gap-4 mb-6">
    <input
      type="text"
      bind:value={newTodo}
      placeholder="New task..."
      onkeydown={(e) => {
        if (e.key === 'Enter' && !e.isComposing) {
          addTodo();
        }
      }}
      class="todo-input"
    />
    <button onclick={addTodo} disabled={!newTodo.trim()} class="todo-add-button">
      Add
    </button>
  </div>

  <ul class="w-full max-w-md">
    {#each todos as todo, i}
      <li class="todo-item">
        <div class="todo-item-body">
          <input type="checkbox" bind:checked={todo.done} class="todo-checkbox" />
          <span class={todo.done ? 'todo-text-done' : 'todo-text'}>{todo.text}</span>
        </div>
        <button onclick={() => deleteTodo(i)} class="todo-delete-button">Delete</button>
      </li>
    {/each}
  </ul>

  {#if todos.length > 0}
    <TodoStats {remainingCount} {completedCount} onClearCompleted={requestClearCompleted} />
  {/if}
</main>

<ConfirmModal
  open={showClearConfirm}
  title="完了タスクの削除"
  message="完了済みのタスクをすべて削除しますか？この操作は元に戻せません。"
  onConfirm={confirmClearCompleted}
  onCancel={cancelClearCompleted}
/>
