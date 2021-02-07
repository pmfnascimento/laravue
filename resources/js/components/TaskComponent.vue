<template>
  <div class="container">

    <ul class="group-list">
          <add-task class="mb-3" @task-added="refresh"></add-task>
      <li class="list-group-item" v-for="task in tasks.data" :key="task.id">
        <a href="#">{{ task.name }}</a>
      </li>
          <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
    </ul>

  </div>
</template>

<script>
export default {
  data(){
    return {
      tasks: {}
    }
  },
  created() {
    axios.get("http://laravue.test/tasksList")
    .then(response => (this.tasks = response.data))
    .catch(error => console.log(error))
  },
  mounted() {
    this.getResults();
  },
  methods: {
		// Our method to GET results from a Laravel endpoint
		getResults(page = 1) {
			axios.get('http://laravue.test/tasksList?page=' + page)
				.then(response => {
					this.tasks = response.data;
				});
		},
    refresh(tasks){
      this.tasks = tasks.data;
    }
	}
};
</script>
