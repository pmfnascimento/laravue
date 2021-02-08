<template>
    <div class="container">

        <ul class="group-list">
            
            <add-task class="mb-3" @task-added="refresh"></add-task>
            <div class="list-group-item">
                <input type="text" class="form-control" @keyup="searchTask" v-model="q" placeholder="Pesquise por uma task">
            </div>
            <li class="list-group-item d-flex justify-content-between align-items-center" v-for="task in tasks.data"
                :key="task.id">
                <a href="#">{{ task.name }}</a>
               <div>
                    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#editModal" @click="getTask(task.id)">
                    Editar
                </button>
                <button type="button" class="btn btn-danger" data-toggle="modal" data-target="#deleteModal" @click="deleteTask(task.id)">
                    Eliminar
                </button>
               </div>
            </li>
            <edit-task v-bind:taskToEdit="taskToEdit" @task-updated="refresh"></edit-task>
            <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                tasks: {},
                taskToEdit: '',
                q: '',
            };
        },
        created() {
            axios
                .get("http://laravue.test/tasksList")
                .then((response) => this.tasks = response.data)
                .catch(error => console.log(error));
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
                    .then(response => this.taskToEdit = response.data)
                    .catch(error => console.log(error));
            },
            deleteTask(id){
              axios.delete("http://laravue.test/tasks/" + id)
                    .then(response => this.tasks = response.data)
                    .catch(error => console.log(error));
            },
            refresh(tasks) {
                this.tasks = tasks.data;
            },
            searchTask(){
                if (this.q.length > 3) {
                    axios.get("http://laravue.test/tasksList/" + this.q)
                    .then(response => this.tasks = response.data)
                    .catch(error => console.log(error));
                }else{
                    axios
                .get("http://laravue.test/tasksList")
                .then((response) => this.tasks = response.data)
                .catch(error => console.log(error));
                }
            }
        },
    };

</script>
