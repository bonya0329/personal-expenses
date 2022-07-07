<template>
    <div>
        <v-list flat>
        <v-subheader>List of all expenses </v-subheader>
        <v-list-item-group
            v-model="selectedItem"
            color="primary"
        >
            <v-list-item
            v-for="(item, i) in Object.keys(items)"
            :key="i"
            >
            <v-list-item-content>
                <v-row justify="center">
                    <v-col cols="1">
                        <v-list-item-title v-text="item">
                        </v-list-item-title>
                    </v-col>
                    <v-col cols="1">
                        <v-btn depressed @click="clear(item)">
                            Clear
                        </v-btn>
                    </v-col>
                    <v-col>
                        <v-btn depressed @click="change_curr(item)">
                            Total amount
                        </v-btn>
                    </v-col>
                </v-row>
                
                

                <div v-for="(el, e) in Object.values(items)" :key="e">
                    <div v-for="(l, k) in el" :key="k"> 
                        <div v-if="l.date == item" class="ml-6">{{l.data}}</div>
                    </div>
                </div>
            </v-list-item-content>

            
            </v-list-item>
            <div align="center"> Total amount {{total}} USD</div>
        </v-list-item-group>
        </v-list>
        
    </div>
</template>

<script>



  export default {
    name: 'List',
  
    data: () => ({
        items: [],
        prev_items: [],
        selectedItem : "",
        fixerData: {},
        to: 'USD',
        from: '',
        amount: '',
        total: 0
    }),
    mounted(){
        var local_items = localStorage.getItem('items')
        console.log(local_items)
        this.items = JSON.parse(local_items)
        this.prev_items = JSON.parse(local_items)
        console.log(this.items)
        if(this.items.length > 0){
            this.items = this.groupArrayOfObjects(this.items,"date")
        }
        
        console.log("grouped items: ",this.items)
        
    },
    methods: {
        change_curr(dtem){
            this.total = 0
            console.log(this.items[dtem])
            var arr = []
            for(let i in this.items[dtem]){
                console.log(this.items[dtem][i].data.split(" ")[0])
                if(this.items[dtem][i].data.split(" ")[1] != 'USD'){
                    this.from = this.items[dtem][i].data.split(" ")[1]
                    this.amount = this.items[dtem][i].data.split(" ")[0]

                    var myHeaders = new Headers();
                    myHeaders.append("apikey", "nYqnHhvqdKhSjIwhtcE4ZTzJDUvBnTtx");

                    var requestOptions = {
                        method: 'GET',
                        redirect: 'follow',
                        headers: myHeaders
                    };
                    //var val = 0
                    console.log("https://api.apilayer.com/fixer/convert?to="+this.to+"&from="+this.from+"&amount="+this.amount+"")
                    fetch("https://api.apilayer.com/fixer/convert?to="+this.to+"&from="+this.from+"&amount="+this.amount+"", requestOptions)
                        .then(response => response.text())
                        .then(result => this.total += Number(JSON.parse(result).result) )
                        .catch(error => console.log('error', error));
                    
                }else{
                    this.total += Number(this.items[dtem][i].data.split(" ")[0])
                }
            }
            console.log(this.total)
            console.log(arr)
        },
        
        groupArrayOfObjects(list, key) {
            return list.reduce(function(rv, x) {
                (rv[x[key]] = rv[x[key]] || []).push(x);
                return rv;
            }, {})
        },
        clear(date){
            
            console.log(date)
            console.log(this.items)
            
            /* if(this.items[date]){
                console.log(this.items.indexOf(this.items[date]))
                //this.items.splice(this.items.indexOf(this.items[date]),1)
            } */
            var local_items = localStorage.getItem('items')
            this.items = JSON.parse(local_items)
            console.log(this.items)
         
            /* for(let i in this.items){
                console.log(this.items[i].date)
                if(this.items[i].date == date){
                    delete this.items[i][date]
                }
            } */
            this.items = this.items.filter(el => {
                return el.date !== date
            })
            
            localStorage.setItem('items', JSON.stringify(this.items))
            if(this.items.length > 0){
                this.items = this.groupArrayOfObjects(this.items,"date")
            }
        }
    }
  }
</script>