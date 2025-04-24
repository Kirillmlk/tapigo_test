<template>
    <h2 class="mb-5 text-xl">Список задач</h2>
    <div class="max-w-7xl mx-auto px-4">
        <table class="min-w-full divide-y divide-gray-200 overflow-x-auto">
            <thead class="bg-gray-50">
            <tr>
                <th scope="col" class="px-6 py-3text-xs text-left font-medium text-gray-500 uppercase tracking-wider">
                    ID
                </th>
                <th scope="col"
                    class="px-6 py-3 text-centre text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Title
                </th>
                <th scope="col"
                    class="px-6 py-3 text-centre text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Done
                </th>
            </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
            <tr v-for="task in tasks" :key="task.id">

                <td class="px-6 py-4 whitespace-nowrap">
                    <div class="flex items-center">
                        <div class="flex-shrink-0 h-10 w-10">
                            <p>{{ task.id }}</p>
                        </div>
                    </div>
                </td>

                <td class="px-6 py-4 whitespace-nowrap">
                    <div class="text-sm text-gray-900">{{ task.title }}</div>
                </td>

                <td class="px-6 py-4 whitespace-nowrap">
                    <input type="checkbox" v-model="task.done" @change="saveTasks"/>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="bg-red-500 border p-2 inline-block rounded mt-8">
        <button class="cursor-pointer text-white " @click="resetTasks">Очистить и перезагрузить</button>
    </div>
</template>

<script>
export default {
    name: 'TaskList',
    data() {
        return {
            tasks: []
        }
    },
    methods: {
        async fetchTasks() {
            const saved = localStorage.getItem('tasks')
            if (saved) {
                this.tasks = JSON.parse(saved)
            } else {
                const res = await fetch('/tasks.json')
                this.tasks = await res.json()
                this.saveTasks()
            }
        },

        saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(this.tasks))
        },

        resetTasks() {
            localStorage.removeItem('tasks')
            location.reload()
        }
    },
    mounted() {
        this.fetchTasks()
    },

}
</script>