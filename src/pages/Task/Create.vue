<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="12" class="text-center">
        <h3>New Task</h3>
      </v-col>
      <v-col cols="12" md="12">
        <v-form
            ref="form"
            v-model="valid"
            lazy-validation
        >
          <v-text-field
              placeholder="Add new task ..."
              outlined
              dense
              :rules="nameRules"
              v-model="task.name"
          ></v-text-field>
          <v-row no-gutters>
            <v-col cols="12">
              <v-subheader>Description</v-subheader>
            </v-col>
            <v-col cols="12">
              <v-textarea
                  outlined
                  dense
                  name="input-7-4"
                  v-model="task.description"
              ></v-textarea>
            </v-col>
          </v-row>
          <v-row no-gutters>
            <v-col cols="6">
              <div class="input-date-picker">
                <v-subheader>Due Date</v-subheader>
                <v-menu
                    v-model="menu2"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    transition="scale-transition"
                    offset-y
                    min-width="290px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                        outlined
                        dense
                        v-model="date"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                      v-model="date"
                      :min="nowDate"
                      @input="menu2 = false"
                  ></v-date-picker>
                </v-menu>
              </div>
            </v-col>
            <v-col cols="6">
              <div class="select-data">
                <v-subheader>Piority</v-subheader>
                <v-select
                    :items="items"
                    outlined
                    dense
                    v-model="task.piority"
                ></v-select>
              </div>
            </v-col>
            <v-col cols="12">
              <v-btn
                  :disabled="!valid"
                  class="btn-add"
                  depressed
                  color="green"
                  @click="submit"
              >
                Add
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import cloneDeep from 'clone-deep'
export default {
  name: 'Create',
  props: ['createTask'],
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    nowDate: new Date().toISOString().substr(0, 10),
    menu2: false,
    modal: false,
    items: ['low', 'normal', 'high'],
    nameRules: [
      v => !!v || 'Name is required',
    ],
    valid: true,
    task: {
      name: '',
      description: '',
      piority: 'normal',
      date: ''
    }
  }),
  methods: {
    submit () {
      const checkValid = this.$refs.form.validate()
      if (checkValid) {
        this.task.date = this.date
        const data = cloneDeep(this.task)
        this.createTask(data)
      }
    },
  },
}
</script>
<style scoped>
  .theme--light.v-subheader {
    padding: 0;
  }
  .v-menu {
    display: block;
  }
  .btn-add.v-btn.v-btn--depressed{
    color: #fff;
    width: 100%;
  }
  .input-date-picker {
    padding-right: 15px;
  }
  .select-data {
    padding-left: 15px;
  }
</style>
<style>
.hello .v-date-picker-table .v-btn.v-btn--active {
  color: #e23b3b;
}
</style>
