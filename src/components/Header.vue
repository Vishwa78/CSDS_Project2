<template>
      <v-app-bar color="primary">
        <!-- Title-->
        <v-toolbar-title
        class="flex text-center" dense>
            <v-app-bar-nav-icon 
              size="32"
            ></v-app-bar-nav-icon>
          {{name}}
          {{ subtitle }}
        </v-toolbar-title>
        <!--Adding Button-->
        <AddButton @task-added="$emit('task-added', task)" addBtn />

      </v-app-bar>
</template>

<script>
import AddButton from '@/components/AddButton.vue'
import Table from '@/components/Table.vue'

  export default {
  props: {
    title: String,
    subtitle: String
  },
  components: {
    AddButton,
  },
  data() {
    return {
      
    }
  },
  emits: ['task-added']
  
}
</script>