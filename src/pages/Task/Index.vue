<template>
  <v-app>
    <v-container>
      <v-card class="hello">
        <v-row>
          <v-col cols="6" md="6">
            <Create :createTask="createTask"></Create>
          </v-col>
          <v-col cols="6" md="6">
            <List :tasks="tasks" :deleteTask="deleteTask" :deleteTasks="deleteTasks"></List>
          </v-col>
        </v-row>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
import Create from "@/pages/Task/Create";
import List from "@/pages/Task/List";

export default {
  name: 'Index',
  data: () => ({
    tasks: []
  }),
  components: {
    Create, List
  },
  methods: {
    createTask (task) {
      task.position = this.tasks.length
      this.tasks.push(task)
    },
    deleteTask (task) {
      this.tasks.splice(task.position, 1)
      this.tasks.forEach( (task, k) => task.position = k)
    },
    deleteTasks (list) {
      if (list.length > 0) {
        list.forEach(l => {
          let key = -1
          this.tasks.forEach( (t, k) => {
            if (t.position === l.position) key = k
          })
          if (key > -1) this.tasks.splice(key, 1)
        })
        this.tasks.forEach( (task, k) => task.position = k)
      }
    }
  },
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
