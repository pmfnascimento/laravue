<template>
  <div class="container">
    <ul class="group-list">
      <li class="list-group-item" v-for="task in tasks.data" :key="task.id">
        <a href="#">{{ task.name }}</a>
      </li>
     
    </ul>
     <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
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
		}
	}
};
</script>
