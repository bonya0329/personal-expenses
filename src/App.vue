<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <span class="mr-2">Personal expenses management web client application </span>

    </v-app-bar>

    

    <v-main>
       <v-bottom-navigation v-model="value">
        <v-btn @click="list">
          <span>List</span>
        </v-btn>

        <v-btn @click="add">
          <span>Add</span>
        </v-btn>
      </v-bottom-navigation>
      <component v-bind:is="currentComponent"></component>
    </v-main>
  </v-app>
</template>

<script>
import Add from './components/Add';
import List from './components/List';

export default {
  name: 'App',

  components: {
    Add,List
  },

  data: () => ({
    currentComponent: Add
  }),
  methods:{
    list(){
      this.currentComponent = List
    },
    add(){
      this.currentComponent = Add
    }
  }
};
</script>
