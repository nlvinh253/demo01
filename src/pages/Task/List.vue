<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="12" class="text-center">
        <h3>To Do List</h3>
      </v-col>
      <v-col cols="12" md="12">
        <v-text-field
            placeholder="Search ..."
            outlined
            dense
            v-model="keySearch"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="12">
        <div v-for="(task, i) in taskShort" :key="i" class="list-task">
            <div class="header-expansion">
              <v-row>
                <v-col cols="8" md="8">
                  <input class="checkbox-dl" type="checkbox" id="vehicle1" name="vehicle1" v-model="selected" :value="task">
                  <span>{{task.name}}</span>
                </v-col>
                <v-col cols="4" md="4">
                  <v-btn
                      class="btn-action"
                      max-width="100"
                      small
                      color="error"
                      dark
                      @click="removeTask(task)"
                  >
                    Remove
                  </v-btn>
                  <v-btn
                      class="btn-action"
                      max-width="100"
                      small
                      color="#4eb5a7"
                      dark
                  >
                    Detail
                  </v-btn>
                </v-col>
              </v-row>
            </div>
        </div>
      </v-col>
      <v-col cols="12" md="12">
        <v-btn
            v-if="selected.length > 0"
            class="btn-action"
            max-width="200"
            width="200"
            small
            color="error"
            dark
            @click="removeTasks"
        >
          Remove
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>

import cloneDeep from 'clone-deep'
export default {
  name: 'List',
  props: {
    tasks: {
      type: Array,
      required: true
    },
    deleteTask: {
      type: Function
    },
    deleteTasks: {
      type: Function
    }
  },
  data: () => ({
    keySearch: '',
    list: [],
    selected: [],
  }),
  computed: {
    taskShort() {
      let listTask = cloneDeep(this.list)
      if (this.keySearch !== '' && listTask.length > 0) {
        listTask = listTask.filter(o => o.name.includes(this.keySearch));
      }
      listTask.sort( (a, b) => {
        return new Date(a.date) - new Date(b.date);
      });
      return listTask
    }
  },
  methods: {
    removeTask (task) {
      if(!confirm("Do you really want delete " + task.name + ' ?')) return false
      this.deleteTask(task)
    },
    removeTasks () {
      if(!confirm("Do you really want delete tasks")) return false
      if (this.selected.length > 0) this.deleteTasks(this.selected)
      this.selected = []
    },
  },
  watch: {
    tasks: {
      deep: true,
      handler(val) {
        this.list = val
      }
    }
  }
}
</script>
<style scoped>
  .btn-action {
    margin: 0 5px;
    float: right;
  }
  .header-expansion {
    border: 1px solid #ddd;
    text-align: left;
    padding: 10px 15px;
    min-height: 50px;

  }
  .list-task {
    margin-bottom: 15px;
  }
  .checkbox-dl {
    margin-right: 10px;
  }
</style>