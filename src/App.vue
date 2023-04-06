<template>
    <main>
        <!-- heading -->
        <header>
            <img src="./assets/pinia-logo.svg" alt="pinia logo">
            <h1> Pinia Tasks </h1>
        </header>

        <!-- new task form -->
        <div class="new-task-form">
            <TaskForm />
        </div>

        <!-- filter -->
        <nav class="filter">
            <button @click="filter = 'all'">All Tasks</button>
            <button @click="filter = 'favs'">Fav Tasks</button>
        </nav>

        <!-- loading -->
        <div class="loading" v-if="isLoading">Loading tasks...</div>

        <!-- task list -->
        <div class="task-list" v-if="filter === 'all'">
            <p >You have {{ totalCount }} tasks</p>
            <div v-for="task in tasks" :key="task.id">
               <TaskDetails :task="task"/>
            </div>
        </div>
        <div class="task-list" v-if="filter === 'favs'">
            <p >You have {{ favCount }} favorites</p>
            <div v-for="task in favs" :key="task.id">
               <TaskDetails :task="task"/>
            </div>
        </div>

        <!-- reset all tasks -->
        <button @click="TaskStore.$reset">Reset</button>
    </main>
</template>

<script>
    import { storeToRefs } from 'pinia';
    import { useTaskStore } from './stores/TaskStore';
    import { ref } from 'vue';
    import TaskForm from './components/TaskForm.vue';
    import  TaskDetails  from './components/TaskDetails.vue';


    export default {
        components: { TaskDetails, TaskForm },
        setup() {
            const TaskStore = useTaskStore()

            const { tasks, isLoading, favs, totalCount, favCount } = storeToRefs(TaskStore)

            // fetch tasks
            TaskStore.getTasks()

            const filter = ref('all')

            return { TaskStore, filter,tasks, isLoading, favs, totalCount, favCount }
        }
    }
</script>