<template>
  <v-container>
        <v-menu
            ref="menu"
            transition="scale-transition"
            offset-y
            min-width="auto"
        >
            <template v-slot:activator="{ on, attrs }">
            <v-text-field
                v-model="date"
                label="Choose date"
                prepend-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                v-on="on"
            ></v-text-field>
            </template>
            <v-date-picker
            v-model="date"
            no-title
            scrollable
            >
            <v-spacer></v-spacer>
            <v-btn
                text
                color="primary"
                @click="menu = false"
            >
                Cancel
            </v-btn>
            <v-btn
                text
                color="primary"
                @click="$refs.menu.save(date)"
            >
                OK
            </v-btn>
            </v-date-picker>
        </v-menu>
    <v-text-field
      v-model="amount"
      label="Amount of money"
    >
    </v-text-field>
    <v-text-field
      v-model="currency"
      label="Currency"
    >
    </v-text-field>
    
    <v-text-field
      v-model="product"
      label="Name of product"
    >
    </v-text-field>

    <v-btn
      class="mr-4"
      @click="submit"
    >
      ADD
    </v-btn>
    
    <div v-show="show_modal">
        <v-alert
       
        border="left"
        close-text="Close Alert"
        color="deep-purple accent-4"
        dark
        dismissible
        >
        You have added a product
        </v-alert>
    </div>
  </v-container>
</template>
<script>
  export default {
    name: 'Add',
    data: () => ({
        items: [],
        new_items: [],
        date: '',
        amount: '',
        currency: '',
        product: '',
        show_modal: false

    }),
    mounted(){
        var local_items = localStorage.getItem('items')
        this.items = JSON.parse(local_items)
    },
    methods:{
        submit(){
            this.show_modal = true
            this.items.push({
                date: this.date,
                data: this.amount+ " " +this.currency+ " " + this.product                
            })
            localStorage.setItem('items', JSON.stringify(this.items))
            console.log("Items: ",this.items)
        },
        groupArrayOfObjects(list, key) {
            return list.reduce(function(rv, x) {
                (rv[x[key]] = rv[x[key]] || []).push(x);
                return rv;
            }, {})
        },
    }
  }
</script>