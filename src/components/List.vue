<template>
    <v-list flat>
      <v-subheader>List of all expenses </v-subheader>
      <v-list-item-group
        v-model="selectedItem"
        color="primary"
      >
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
        >
          <v-list-item-content>
            <v-list-item-title v-text="item.amount"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
</template>

<script>
  export default {
    name: 'List',

    data: () => ({
        items: []
    }),
    methods:{
        clear(){
            
        }
    },
    mounted(){
        var local_items = localStorage.getItem('items')
        console.log(local_items)
        this.items = JSON.parse(local_items)
        
        console.log(this.items)
    }
  }
</script>