<template>
  <div class="todo-container">
    <h1 class="todo-title">Todo List</h1>
    <div>
      <img
        class="todo-loading"
        src="https://media.istockphoto.com/id/1335247217/vector/loading-icon-vector-illustration.jpg?s=612x612&w=0&k=20&c=jARr4Alv-d5U3bCa8eixuX2593e1rDiiWnvJLgHCkQM="
        alt=""
        v-if="toDos.length<5"
      />
    </div>
    <div>
      <div v-for="(getData, index) in toDos" :key="index">
        <div
          class="item"
          :class="{ finsh: getData.completed == true }"
          @dblclick="toggleTask(index)"
        >
          <p>{{ getData.title }}</p>
          <img
            class="removeForm-img"
            src="https://png.pngtree.com/png-clipart/20190517/original/pngtree-vector-cross-icon-png-image_4267399.jpg"
            alt=""
            @click="removeTodo(index)"
          />
        </div>
      </div>
      <img
        class="addForm-img"
        src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQupKxBOXvQZjiLc4OO7qkCq4ZH3bgIg0xjPxi9nolwKUcBgxXEEbmg4anrlkDokpNYEhs&usqp=CAU"
        alt=""
        @click="appearForm"
      />
      <div
        class="form-container"
        v-if="status === 'default'"
        @offForm="offForm"
      >
        <input type="text" v-model="toDoName" 
          @keyup="addToDoEnter"
        />
        <div class="two-button">
          <button class="cancel-button" @click="offForm">Cancel</button>
          <button class="add-button" @click="addTodo">Add work</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      toDoName: "",
      toDos: [],
      status: "none",
    };
  },
  created(){
    const getTodo = async () => {
      try {
        const res = await axios.get(
          "https://jsonplaceholder.typicode.com/todos?_start=0&_limit=5&_delay=3000"
        );
        this.toDos = res.data;
        return (this.toDos)
      } catch (error) {
        console.log(error);
      }
    };
    getTodo();
  },
  //Tích 2 lần thể hiện hoàn thành công việc
  methods: {
    toggleTask(index) {
      if (this.toDos[index].completed == false) {
        this.toDos[index].completed = true;
        console.log(this.toDos[index].completed);
      } else {
        this.toDos[index].completed = false;
      }
    },
    //Thêm work
    addTodo() {
      if (this.toDoName == "") {
        alert("Type work");
      } else {
        this.toDos.push({
          userId: 1,
          id: this.toDos.length + 1,
          title: this.toDoName,
          completed: false,
        });
      }
      this.toDoName = "";
    },
    //thêm work bằng dấu Enter
    addToDoEnter(event){
      if(event.key == "Enter"){
        this.toDos.push({
          userId: 1,
          id: this.toDos.length + 1,
          title: this.toDoName,
          completed: false,
        })
        this.toDoName = "";
      }
    },
    //Xóa work
    removeTodo(index) {
      let choice = confirm("Delete Todo");
      if (choice == true) {
        this.toDos.splice(index, 1);
      } else {
        console.log("Bạn đã hủy xóa list");
      }
    },
    //Hiện form
    appearForm() {
      this.status = "default";
    },
    //Xóa form
    offForm() {
      this.status = "none";
    },
  },
};
</script>
<style scoped>
.todo-container {
  width: 600px;
  height: 100%;
  margin: auto;
  border: solid 2px black;
  align-items: center;
  align-content: center;
}
.todo-loading {
  width: 100px;
  height: 100px;
}
.item {
  display: flex;
  position: relative;
  justify-content: space-around;
  width: 500px;
  border: solid 2px black;
  margin-top: 20px;
  border-radius: 50px;
  margin-left: auto;
  margin-right: auto;
  cursor: pointer;
}
.item.finsh {
  border: solid 2px greenyellow;
}
.item:hover {
  background-color: rgb(220, 220, 220);
}
.addForm-img {
  width: 50px;
  height: 50px;
  margin-top: 10px;
  cursor: pointer;
}
.removeForm-img {
  position: absolute;
  left: 470px;
  top: 15px;
  width: 20px;
  height: 20px;
  display: none;
  cursor: pointer;
}
.item:hover > .removeForm-img {
  display: block;
}
input {
  width: 500px;
  height: 50px;
  border-radius: 10px;
}
.two-button {
  display: flex;
  justify-items: center;
  margin-left: 200px;
  margin-top: 30px;
}
.cancel-button {
  display: block;
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  cursor: pointer;
}
.cancel-button:hover {
  cursor: pointer;
  opacity: 0.5;
}
.add-button {
  display: block;
  margin-left: 20px;
  background-color: #1ba1e2;
  color: white;
  border-radius: 10px;
  padding: 10px 10px;
  cursor: pointer;
}
.add-button:hover {
  cursor: pointer;
  opacity: 0.5;
}
</style>
