<template>
<v-card>
   <v-dialog v-model="showDialog">
       <template v-slot:activator="{ props }">
        <v-btn style="background-color: #2064c4; color: white"
            @click="click"
            >
        <v-icon>{{addBtn ? 'mdi-plus-circle' : 'mdi-pencil-box-outline'}} </v-icon> {{ this.btn_title }} </v-btn>
        </template>

<!-- Name of Dialog -->
       
           <v-card class="dialog">
               <v-card-title class=dialog_title style="background-color: #2064c4; color: white">
                <v-icon size="x-small" class="inline-card__icon" >{{this.addBtn ? 'mdi-plus-circle' : 'mdi-pencil-box-outline'}}</v-icon>
                     {{ this.card_title }} 
               </v-card-title>
           
            <v-container>
<!-- Dialog title -->
        <v-row v-if="this.addBtn">
            <v-col>
         <v-text-field class="input" v-model="name" :rules="[titlerules.required]" label="Title"></v-text-field>
            </v-col>
        </v-row>

<!-- Dialog description -->
        <v-row>
            <v-col>
         <v-text-field class="input" v-model="desc" :rules="[descrules.required]" label="Description" v-if="addBtn || currentTask"></v-text-field>
            </v-col>
        </v-row>

            <!-- Dialog deadline (date picker) -->
            <v-row>
                <v-col>
               <v-text-field class="input" v-if="addBtn || currentTask" type="date" label="Deadline" v-model="due" :rules="[v => !!v || 'Deadline is required']"></v-text-field>
                </v-col>
            </v-row>

        <!-- Dialog priority -->
        <v-row>
            <v-col>
           <v-radio-group label="Priority" inline v-model="priority" :rules="[v => !!v || 'Select an option']" style="margin-left:10px;" v-if="addBtn || currentTask">
               <v-radio label="Low" value="low"></v-radio>
               <v-radio label="Med" value="med"></v-radio>
               <v-radio label="High" value="high"></v-radio>
           </v-radio-group>
            </v-col>
        </v-row>

            </v-container>
         <v-card-actions>
               <v-btn class="custom-btn" variant="tonal"  @click="publish" style="background-color: #1976d2;" color="white"><v-icon color="white">{{this.addBtn ? 'mdi-plus-circle' : 'mdi-pencil-box-outline'}}</v-icon>{{this.publish_title}}</v-btn>
               <v-btn @click="cancel" style="background-color: red;" color="white"><v-icon>mdi-cancel</v-icon>  Cancel</v-btn>
        </v-card-actions>


        <hr />
           </v-card>
 </v-dialog>

 <v-snackbar v-model="showSnackbar" :timeout="timeout" color= "green" location= "bottom right">
      {{ snackbarText }}
    </v-snackbar>
</v-card>
</template>


<script>

export default {
    name: 'AddButton',
    data() {
       return {
           titlerules: {
               required: this.addBtn && (value => !!value || 'Title is required!'),
            },
            descrules: {
               required: value => !!value || 'Description is required!',
            },
          //  showSnackbar: false,
           showDialog: false,
           card_title:"",
           publish_title:"",
           btn_title:"",
           name: "",
           desc: "",
           priority: null,
           due: null,

           //snackbar properties
           showSnackbar: false,
           snackbarText: "",
           timeout: 4000
       };
   },
   methods: {
       click() {
        if(this.addBtn) {
           this.showDialog = true;
           }
           else {
               this.desc = this.currentTask.desc
               this.due = this.currentTask.due
               this.priority = this.currentTask.priority
               this.showDialog = true;
           }
       },
       cancel() {
           this.showDialog = false
           this.name = ''
           this.desc = ''
           this.due = ''
           this.priority = ''
       },

       publish(e) {
           e.preventDefault()
           if(!this.addBtn) {
            if(!this.desc || !this.due || !this.priority) {
            return
            }
               this.showDialog = false
               const update_task = {
                   name: this.edit_title,
                   desc: this.desc,
                   due: this.due,
                   priority: this.priority
               };
          //     this.$emit("set-snackbar-updated", true)
            //   this.$emit("task-edited", update_task)
               this.desc = ''
               this.due = null
               this.priority = ''
               this.snackbarText = "Task was updated successfully!";
               this.showSnackbar = true;
                }
           else {

               //add title unique stuff
           }
            if(!this.name || !this.desc || !this.due || !this.priority) {
            return
            }
           this.showDialog = false;
           const newTask = {
               name: this.name,
               desc: this.desc,
               due: this.due,
               priority: this.priority
           }
           this.$emit("task-added", newTask);
           this.name = ''
           this.desc = ''
           this.due = null
           this.priority= ''
           this.snackbarText = "Task was added successfully!";
           this.showSnackbar = true;
       }
   },
   props: {
       addBtn: Boolean,
       edit_title: String,
       taskList: Array,
       add: Function,
       currentTask: Object,
   },
   created() {
       if(this.addBtn) {
           this.card_title = "Add Task",
           this.publish_title = "Add",
           this.btn_title = "Add"
       }
       {
           if(!this.addBtn) {
               this.card_title = "Edit Task",
               this.publish_title = "Edit",
               this.btn_title = "Update"
           }
       }
   }
}
</script>


<style>
.v-card-actions {
 justify-content: flex-end;
}
.custom-btn {
   background-color: #1976d2;
   color: white;
}
.input {
   margin-top: 20px;
   margin-left: 20px;
   margin-right: 20px;
}
.priority_subtext {
   font-size: 15px;
   margin-left: 20px;
}
.overlay {
 position: fixed;
 z-index: 9999;
 top: 0;
 left: 0;
 width: 100%;
 height: 100%;
 background-color: rgba(0, 0, 0, 0.5);
 display: flex;
 justify-content: center;
 align-items: center;
}


.dialog {
 background-color: white;
 max-width: 300px;
 min-height: 350px;
 border-radius: 20px;
 margin-top: 0;
 top: 50%;
    left: 40%;
    
 position: absolute;
 box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3);
}


.dialog_title {
 font-weight: bold;
 font-size: 15px;
 color:#2196F3;
}


.dialog_description {
 font-size: 16px;
}

.v-snack_content {
    background-color: green;
    bottom: 0;
    right: 0;
}


</style>
