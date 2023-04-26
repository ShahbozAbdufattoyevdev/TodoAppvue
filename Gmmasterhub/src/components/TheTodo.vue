<template>
    <h1><span>GM</span> Uzbekistan</h1>
    <div class="container">
        <div class="category">
            <h2>Tact Category</h2>
            <ol>
                <li>Booking</li>
                <li>Applications</li>
                <li>Offers</li>
            </ol>
            <ul class="set">
                <li>Settings</li>
                <li>Themes</li>
            </ul>
        </div>
        <div class="addbox">
            <h2>Booking list</h2>
            

            <div class="todo">
              <div>
                <input
                        type="text"
                        placeholder="Add tasks..."
                        class="add_task"
                        :class="currentToDoId !== null ? 'focus:border-green-400' : ''"
                        v-model.trim="value"
                        @keyup.enter="addTodo"
                    >
                    <button v-if="currentToDoId === null"  @click="addTodo" class="btn">Add</button>
                    <button v-else @click="editedTodo" class="btn">edit</button>
              </div>
              <div class="search" v-if="todoList.length">
                    <input
                        type="text"
                        placeholder="Search..."
                        v-model.trim="search"
                        @keypress.enter="searchTodo"
                    >
                </div>
                <div v-else>
                    <h2 style="text-align: center;">Empty yet</h2>
                </div>
              <div class="items" v-for="(item, index) in todoList" :key="index">
                <div>
                    <span style="font-size: 20px;">{{ index }}. </span>
                    <span>{{ item.title }}</span>
                </div>
                <div>
                 
                    <input 
                         
                        v-model="item.checked"
                        class="checkbox" 
                        type="checkbox"
                    >
                    <button @click="removeTodo(item)"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: rgba(0, 255, 255, 1);transform: ;msFilter:;"><path d="M5 20a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V8h2V6h-4V4a2 2 0 0 0-2-2H9a2 2 0 0 0-2 2v2H3v2h2zM9 4h6v2H9zM8 8h9v12H7V8z"></path><path d="M9 10h2v8H9zm4 0h2v8h-2z"></path></svg></button>
                    <button @click="updateTodo(item)"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style="fill: rgba(0, 255, 255, 1);transform: ;msFilter:;"><path d="M19.045 7.401c.378-.378.586-.88.586-1.414s-.208-1.036-.586-1.414l-1.586-1.586c-.378-.378-.88-.586-1.414-.586s-1.036.208-1.413.585L4 13.585V18h4.413L19.045 7.401zm-3-3 1.587 1.585-1.59 1.584-1.586-1.585 1.589-1.584zM6 16v-1.585l7.04-7.018 1.586 1.586L7.587 16H6zm-2 4h16v2H4z"></path></svg></button>
                </div>
                
            </div>
            <div class="clearList">
                    <div>
                        <h3>Acsepted Booking: {{ countSelected }} </h3><br>
                    <input
                        @click="selectAll"
                        class="checkbox" 
                        name="" 
                        type="checkbox" 
                        id="selectalltodo"
                        v-model="selectAllTodo"
                    >
                     <label for="selectalltodo"> Accept all</label><br>
                    </div>
                    <div class="radio">
                        <input
                            @change="test(allTodo)" 
                            type="radio" 
                            checked
                            name="accepted" 
                            id="alltodo"
                        >
                        <label for="all"> All</label><br>
                        <input
                            @change="test(accTodo)" 
                            type="radio" 
                            name="accepted" 
                            id="acceptedtodo"
                        >
                        <label for="acc"> Accepted</label><br>
                        <input
                            @change="test(nAcceptTodo)" 
                            type="radio" 
                            name="accepted"
                            id="naccepttodo"
                        >
                            
                        <label for="notacc"> Not Accepted</label><br>
                    </div>
                    <button class="clearbutton" @click="clearSelected"> Clear selected</button>
                </div>
            </div>
            
             </div>
    </div>
</template>

<script>

export default {
    name: 'TheTod',
    data(){
        return {
            value: '',
            // idCreator: 0,
            todoList: [],
            currentToDoId: null,
            selectAllTodo: false,
            dataFilter: null,
            allTodo:'all',
            accTodo: 'acc',
            nAcceptTodo: 'notacc',
            currentToDoList: [],
            search: '',
            searchListTodo:[]
        }
    },
    methods: {
        searchTodo(){
            // this.searchListTodo = this.todoList.filter(res => res.title.includes(this.search))
            this.searchListTodo = [...this.todoList]
            this.todoList = []
            for (let i = 0; i < this.searchListTodo.length;i++){
                if(this.searchListTodo[i].title.includes(this.search)){
                    this.todoList.push(this.searchListTodo[i])
                }
            }
        },
         addTodo() {
            // this.idCreator = this.todoList.length
            if(this.value.length){
                this.value = this.value[0].toUpperCase(0) + this.value.slice(1)
                this.todoList.push({
                id: this.idCreator,
                title: this.value,
                checked: false
            })
            }
                this.value = ''
                this.idCreator++
         },
         removeTodo(item) {
            const index = this.todoList.findIndex(el => el.id === item.id );
           let del = prompt('Are you sure delete this command, yes or no').toLowerCase()
           if(del == 'yes'){
            this.todoList.splice(index, 1)
         } else{
            alert('Try again or update toDo')
         }
        },
        updateTodo(item){
            if(!this.value.length){
                this.value = item.title
                this.currentToDoId = item.id
            }
        },
        editedTodo() {
            const index = this.todoList.findIndex(el => el.id === this.currentToDoId );
            this.todoList[index].title = this.value
            this.currentToDoId = this.value
            this.value = this.currentToDoId = null
        },
        clearSelected() {   
        this.todoList = this.todoList.filter(el => el.checked === false)
        },
       async selectAll(){
            this.selectAllTodo = !this.selectAllTodo
            if(this.selectAllTodo){
                this.todoList.map(el => {el.checked = true})
            } else {
                this.todoList.map(el => {el.checked = false})
            }
        },
        test(arg){
            if(arg == this.allTodo){
            this.todoList = this.currentToDoList
            } else if(arg == this.accTodo){
                this.todoList = this.currentToDoList.filter(el => el.checked === true)
            }else if(arg == this.nAcceptTodo){
                this.todoList = this.currentToDoId.filter(el => el.checked === false)
            }
        }
    },
    mounted(){
        this.idCreator = this.todoList.length
    },
    computed: {
        countSelected() {
           return this.todoList.filter(el => el.checked === true).length
        }
    }
    
}
// let itemLength = document.querySelector('items').length
// document.getElementById('elLength').innerHTML = itemLength

</script>

<style scoped> 
li{
    cursor: pointer;
}
h1{
    text-align: center;
    margin:10px auto;
    width: 20%;
}
h1 span{
    text-decoration: underline;
     background-color: rgb(8, 8, 136);
    padding: 2px 10px;
}
h1 span:hover{
    box-shadow: 0 0 10px white;
}
.container{
    width: 1200px;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    color: aqua;
}
.category{
    width: 50%;
    height: 600px;
    border-top-left-radius:20px ;
    border-bottom-left-radius:20px ;
    border:2px solid aqua ;
    border-right:none ;
}
h2{
    text-align: center;
    margin-top: 10px;
}
.addbox{
    width: 50%;
    height:600px;
    border-top-right-radius:20px ;
    border-bottom-right-radius:20px ;
    border:2px solid aqua ;
   
}
ol li{
    font-size: 20px;
}
ol li:hover{
    text-decoration: underline;
}
.set{
    margin-top: 380px;
    line-height: 30px;
    text-decoration: underline;
}
.set li{
 list-style: none;
}

/* category page end */

/* booking page start  */

.search{
 margin-left: 18%;
 margin-top: 10px;
}
.search input{
    width: 400px;
    height: 40px;
    padding: 10px;
    outline: none;
    border-radius:8px;
    transition: all 0.3s;
    color: white;
    background-color: rgb(64, 61, 61);
}
.search input:focus{
    border: aqua 2px solid;
}
.add_task{
    width: 400px;
    height: 40px;
    padding: 10px;
    outline: none;
    border-radius:8px;
    transition: all 0.3s;
    color: white;
    background-color: rgb(64, 61, 61);
    margin: auto;
}
.todo{
    width: 100%;
}   
.add_task{
    margin-left: 18%;
    margin-top: 10px;
    /* margin:10px  110px; */
}
.add_task:focus{
    border-color: aquamarine;
}

.btn{
    width: auto;
    height: 40px;
    background: transparent;
    border: 2px solid aliceblue;
    font-size: 15px;
    position: relative;
    right: 42px;
    border: none ;
    color: aqua;
}
.btn:hover{
    border:2px solid aqua;
    border-radius: 8px;
    color: aqua;
    box-shadow: inset 0 0 10px aqua;
}
.items{
    width: 95%;
    border-bottom:solid white 2px;
    display: flex;
    justify-content: space-between;
    margin: auto;
}
.items button{
background: transparent;
border: none;
}
.clearList{
    width:  600px;
   
  display: flex;
  justify-content: space-between;
    position: fixed;
    bottom:30px;    
    align-items: center; 
    margin-left: 20px;
}
.clearbutton{
    padding: 8px 25px;
    background: transparent;
    border: aqua solid 2px;
    color: aqua;
    border-radius: 8px;
    right: 8%;
}
.clearList input{
    margin-right: 5px;
    width: 15px;
 height:15px;
}
.clearList button:hover{
    border:2px solid aqua;
    border-radius: 8px;
    color: aqua;
    box-shadow: inset 0 0 10px aqua;
}


 
</style>