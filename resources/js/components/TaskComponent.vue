<template>
    <div class="container">
        <ul class="group-list">
            <add-task class="mb-3" @task-added="refresh"></add-task>
            <li class="list-group-item d-flex justify-content-between align-items-center" v-for="task in tasks.data"
                :key="task.id">
                <a href="#">{{ task . name }}</a>
                <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#editModal" @click=getTask(task.id)>
                    Editar
                </button>
            </li>
            <edit-task v-bind:taskToEdit="taskToEdit"></edit-task>
            <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                tasks: {},
                taskToEdit: ''
            };
        },
        created() {
            axios
                .get("http://laravue.test/tasksList")
                .then((response) => (this.tasks = response.data))
                .catch((error) => console.log(error));
        },
        mounted() {
            this.getResults();
        },
        methods: {
            // Our method to GET results from a Laravel endpoint
            getResults(page = 1) {
                axios
                    .get("http://laravue.test/tasksList?page=" + page)
                    .then((response) => {
                        this.tasks = response.data;
                    });
            },
            getTask(id){
              axios.get("http://laravue.test/tasks/edit/" + id)
                    .then((response) => this.taskToEdit = response.data.name)
                    .catch((error) => console.log(error));
            },
            refresh(tasks) {
                this.tasks = tasks.data;
            },
        },
    };

</script>
