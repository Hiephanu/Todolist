<template>
  <div class="todo-container">
    <h1 class="todo-title">Todo List</h1> 
    <div v-for="(getData, index) in getDatas" :key="index">
      <div class="item" :class="{finsh:getData.completed==true}" @dblclick="finishTask(index)">
        <p>{{ getData.title }}</p>
        <img class="removeForm-img" src="https://png.pngtree.com/png-clipart/20190517/original/pngtree-vector-cross-icon-png-image_4267399.jpg" alt="" @click="removeTodo(index)">
      </div>
    </div>
    <img class="addForm-img" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQupKxBOXvQZjiLc4OO7qkCq4ZH3bgIg0xjPxi9nolwKUcBgxXEEbmg4anrlkDokpNYEhs&usqp=CAU" alt="" @click="appearForm">
    <div class="form-container" v-if="status==='default'"  @offForm="offForm">
    <input type="text" v-model="message">
        <div class="two-button">
            <button class="cancel-button" @click="offForm">Cancel</button>
            <button class="add-button" @click="addTodo"> Add work</button> 
        </div>
   </div>
  </div>
  
</template>
<script>
 import axios from 'axios'
//  import ref from 'vue'
  export default{
    data(){
      return {
        message:'',
        getDatas:[],
        status:'none',
        number:0,
      }
    },
    mounted(){
      //lấy dữ liệu từ api trả về
      const getTodo= async ()=>{
          try {
            const res=await axios.get('https://jsonplaceholder.typicode.com/todos?_start=0&_limit=5&_delay=3000')
            this.getDatas= res.data
          }
          catch(error){
            console.log(error);
          }
        }
        getTodo()
      },
    //Tích 2 lần thể hiện hoàn thành công việc
    methods:{
      finishTask(index){
        if(this.getDatas[index].completed==false)
        {
          this.getDatas[index].completed=true
          console.log(this.getDatas[index].completed);
        }
        else {
          this.getDatas[index].completed=false
        }
       },
      //Thêm work
      addTodo(){
        if(this.message==""){
          alert('Type work')
        }
        else {
          this.getDatas.push(
            {
              userId:1,
              id:this.getDatas.length+1,
              title:this.message,
              completed:false
            }
          )
        }
        this.message=''
      },
      //Xóa work
      removeTodo(index){
        let choice=confirm('Delete Todo')
        if(choice==true){
        this.getDatas.splice(index,1)
        }
        else {
          console.log("Bạn đã hủy xóa list");
        }
      },
      //Hiện form
      appearForm(){
        this.status="default"
      },
      //Xóa form
      offForm(){
        this.status="none"
      },
    }
  }
</script>
<style scoped>
.todo-container{
  width:600px;
  height:100%;
  margin:auto;
  border:solid 2px black;
  align-items: center;
  align-content: center;
}
.item{
  display:flex;
  justify-content: space-around;
  width:500px;
  border:solid 2px black;
  margin-top:20px;
  border-radius:50px;
  margin-left:auto;
  margin-right:auto;
}
.item.finsh{
  border:solid 2px greenyellow;
}
.item:hover{
  background-color:rgb(220,220,220);
}
.addForm-img{
  width:50px;
  height:50px;
  margin-top:10px;
}
.removeForm-img{

  left:73%;
  margin-top:17px;
  width:  20px;
  height: 20px;
  display:none;
}
.item:hover>.removeForm-img{
  display:block;
}
input{
    width:500px;
    height:50px;
    border-radius:10px;
}
.two-button{
    display:flex;
    justify-items: center;
    margin-left:200px;
    margin-top:30px;
}
.cancel-button{
    display:block;
    background-color: white;
    border-radius: 10px;
    padding:10px;
}
.cancel-button:hover{
    cursor: pointer;
    opacity: 0.5;
}
.add-button{
    display:block;
    margin-left:20px;
    background-color: #1ba1e2;
    color:white;
    border-radius: 10px;
    padding:10px  10px;
}
.add-button:hover{
    cursor: pointer;
    opacity: 0.5;
}
</style>