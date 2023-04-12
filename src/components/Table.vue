<template>
 <v-table class="mt-3" style="margin:30px">
     <template v-slot:default>
   <thead>
     <tr>
       <th style="width:calc(100%/6);">Title</th>
       <th style="width:calc(100%/6);">Description</th>
       <th style="width:calc(100%/6);">Deadline</th>
       <th style="width:calc(100%/6);">Priority</th>
       <th style="width:calc(100%/6);">Is Complete</th>
       <th style="width:calc(100%/6);">Action</th>
     </tr>
   </thead>
   <tbody>
     <tr v-for="task in tasks" :key="task.name" height="75px" >
       <td>{{ task.name }}</td>
       <td>{{ task.desc }}</td>
       <td> {{ formatDate(task.due) }} </td>
       <td> {{ task.priority }} </td>
       <td> <div> <v-checkbox class="d-flex" v-model="task.checkbox">
           </v-checkbox> </div> </td>


       <td> 
           <v-row v-if="!task.checkbox">
           <AddButton @task-edited="updateTask" :currentTask="task" :edit_title="task.name"/> </v-row> 
           <v-row>
           <DelButton @task-deleted="deleteTask" :del_title="task.name" /> </v-row>
        </td>
            

     </tr>
   </tbody>
     </template>
 </v-table>

    <v-snackbar v-model="showSnackbar" :timeout="timeout" color= "green" location= "bottom right" style="z-index: 9999;">
      Task was deleted successfully!
    </v-snackbar>

</template>


<script>
import AddButton from './AddButton.vue';
import DelButton from './DelButton.vue';
import moment from 'moment';
import VueMoment from 'vue-moment';

 export default {

     data() {
         return {
             checkbox: false,
            showSnackbar: false,
           timeout: 4000
         }
     },
     methods: {
        updateTask(updateTask) {
        for (let i = 0; i < this.tasks.length; i++) {
          if (this.tasks[i].name == updateTask.name) {
            this.tasks[i] = {
              name: this.tasks[i].name,
              desc: updateTask.desc,
              due: updateTask.due,
              priority: updateTask.priority,
            };
          }
        }
        },
        formatDate(due) {
            if(!(due == "")) {
                return moment(due).format('MM/DD/YYYY')
            }
            else {
                return ""
            }
        },
        deleteTask(name) {
            for (let i = 0; i < this.tasks.length; i++) {
             if (this.tasks[i].name == name) {
                 this.tasks.splice(i, 1);
                 this.$emit('task-deleted', name);
          }
        }
        this.showSnackbar = true;
        }
     },
   props: {
       tasks: Array,
   },
   components: {
       AddButton,
       DelButton
   }
 }
</script>

