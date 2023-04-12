<template>
  <v-app>
    <v-card class="mt-9">
     <v-card-title>
        <v-app-bar
         color="primary"
          density="compact">
              <!-- Header -->
          <v-toolbar-title class="flex text-center">
           <v-app-bar-nav-icon size="30"></v-app-bar-nav-icon>
              FRAMEWORKS
             {{subtitle}}
         </v-toolbar-title>
          <AddButton @task-added="addTask" :taskList="this.tasks" addBtn/>
        </v-app-bar>
     </v-card-title>
          <!-- Header End -->

          <!-- AddButton Box -->

          <!-- Table -->
      <v-container fluid>
        <Table :tasks="tasks"> </Table>
      </v-container>
      </v-card>

          <!-- Table End -->

  </v-app>
</template>

<script>
import Table from './components/Table.vue';
import AddButton from './components/AddButton.vue';
import Header from './components/Header.vue';

export default {
  name: 'App',
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    addTask(newTask) {
      this.tasks.push(newTask)
    }, 
    delTask(){
        this.tasks = this.tasks.filter((task) => task.name !== name)
      },
  },
  components: {
    Table,
    AddButton,
    Header
  }
}
</script>
