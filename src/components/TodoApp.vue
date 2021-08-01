
 
<template>
  <div class="container">
    <p>My Vue Todo App</p>
    <input type="text" v-model="task" placeholder="Type here..." />

    <button class="addButton" v-on:click="addValue()">Add</button>
    <br /><br />
    

    <table>
      <tr style="color: orange; background-color: blue">
        <th>Task Name</th>
        <th>Edit</th>
        <th>Delete</th>
        <th>Status</th>
      </tr>
      <!-- <td>Buy vegetable</td> -->

      <tr v-for="(t, index) in filteredList" v-bind:key="index">
        <td>
          <span v-if="!t.isEdit" class="taskvalue"
            >{{ t.taskName }}</span
          >

          <span v-if="t.isEdit"
            ><input
              type="text"
              v-model="eTask"
              v-on:keyup.enter="editTaskValue()"
              class="editInput"
          /></span>
        </td>

        <td>
          <button class="editButton" v-on:click="editTask(index)">Edit</button>
        </td>
        <td>
          <button class="deleteButton" v-on:click="deleteTask(index)">
            Delete
          </button>
        </td>
        <td>
          <input
            type="checkbox"
            v-model="t.status"
            v-on:click="taskStatusModifier(index)"
          />
        </td>
      </tr>
    </table>

    <div class="filterBox">
      <input
        type="checkbox"
        id="all"
        value="all"
        v-on:click="allFilter()"
        :checked="isall"
      />
      <label for="all">All</label>

      <input
        type="checkbox"
        id="completed"
        value="completed"
        v-on:click="completedFilter()"
        :checked="iscompleted"
      />
      <label for="completed">Completed</label>

      <input
        type="checkbox"
        id="notcompleted"
        value="notcompleted"
        v-on:click="notCompletedFilter()"
        :checked="isnotcompleted"
      />
      <label for="notcompleted">Not Completed</label>

      <input
        type="checkbox"
        id="due"
        value="due"
        v-on:click="dueFilter()"
        :checked="isdue"
      />
      <label for="due">Due</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "MainComponent",
  data() {
    return {
      task: "",
      eTask: "",
      dueDate: "",
      diffBDate: 0,
      dueTimeType: "",
      msgForUser: "",
      editTaskIndex: null,
      filter: "all",
      allList: true,
      toDoList: [],
      filteredList: [],
      isall: true,
      iscompleted: false,
      isnotcompleted: false,
      isdue: false,
    };
  },
  methods: {
    editTaskValue() {
      if (this.eTask.length > 0) {
        this.msgForUser = "";
        this.toDoList[this.editTaskIndex].taskName = this.eTask;
        this.eTask = "";
        this.toDoList[this.editTaskIndex].isEdit = false;
        this.editTaskIndex = null;
        this.updateList();
      } else {
        this.msgForUser = alert(
          "Can not add empty field to task, please type something"
        );
      }
    },
    addValue() {
      if (this.task.length > 0) {
        {
          this.msgForUser = "";

          this.toDoList.push({
            taskName: this.task,
            dueDat: this.diffBDate,
            dueType: this.dueTimeType,
            isEdit: false,
            status: false,
          });
          this.task = "";
          this.dueDate = "";
          console.log(this.toDoList);
          this.updateList();
        }
      } else {
        this.msgForUser = alert("please type here");
      }
      //    else {
      //     this.msgForUser =
      //       "Can not add empty field to task, please type something";
      //   }
    },
    deleteTask(index) {
      this.toDoList.splice(index, 1);
      this.updateList();
    },
    editTask(index) {
      this.eTask = this.toDoList[index].taskName;
      this.editTaskIndex = index;
      this.toDoList[index].isEdit = true;
    },
    taskStatusModifier(index) {
      if (this.toDoList[index].status == true) {
        this.toDoList[index].status = false;
      } else {
        this.toDoList[index].status = true;
      }
    },
    updateList() {
      this.filteredList = [];
      const len = this.toDoList.length;
      var i = 0;
      if (this.filter == "all") {
        for (i = 0; i < len; i++) {
          this.filteredList.push(this.toDoList[i]);
        }
      } else if (this.filter == "completed") {
        for (i = 0; i < len; i++) {
          if (
            this.toDoList[i].status == true &&
            this.toDoList[i].dueType != "Expired"
          ) {
            this.filteredList.push(this.toDoList[i]);
          }
        }
      } else if (this.filter == "notcompleted") {
        for (i = 0; i < len; i++) {
          if (
            this.toDoList[i].status == false &&
            this.toDoList[i].dueType != "Expired"
          ) {
            this.filteredList.push(this.toDoList[i]);
          }
        }
      } else if (this.filter == "due") {
        for (i = 0; i < len; i++) {
          if (this.toDoList[i].dueType == "Expired") {
            this.filteredList.push(this.toDoList[i]);
          }
        }
      }
    },
    allFilter() {
      this.filter = "all";
      this.isall = true;
      this.iscompleted = false;
      this.isnotcompleted = false;
      this.isdue = false;
      this.updateList();
    },
    completedFilter() {
      this.filter = "completed";
      this.isall = false;
      this.iscompleted = true;
      this.isnotcompleted = false;
      this.isdue = false;
      this.updateList();
    },
    notCompletedFilter() {
      this.filter = "notcompleted";
      this.isall = false;
      this.iscompleted = false;
      this.isnotcompleted = true;
      this.isdue = false;
      this.updateList();
    },
    dueFilter() {
      this.filter = "due";
      this.isall = false;
      this.iscompleted = false;
      this.isnotcompleted = false;
      this.isdue = true;
      this.updateList();
    },
  },
  mounted() {
    this.updateList();
  },
};
</script>

<style scoped>
input {
  height: 30px;
  width: 40%;
  border: 1px solid #d9d9d9;
  outline: none;
}
.taskvalue {
}
input.dueDate {
  height: 33px;
  width: 15%;
  margin-left: 1%;
  margin-right: 1%;
}
button.addButton {
  border-radius: 5px;
  outline: none;
  height: 34px;
  color: white;
  background-color: blue;
  width: 70px;
}
table {
  border: 2px solid black;
  width: 500px;
  margin: 1% auto;
  margin-bottom: 50px;
}
table tr td {
  border: 1px solid black;
}
.editInput {
  width: 70%;
}
.editButton {
  background-color: blue;
  color: black;
  border-radius: 5px;
  outline: none;
  height: 30px;
  width: 80px;
}
.deleteButton {
  background-color: blue;
  color: black;

  border-radius: 5px;
  outline: none;
  height: 30px;
  width: 80px;
}
/*Filter box design*/
.filterBox {
  height: 50px;
  background-color: rgb(228, 165, 78);
  color: black;
  font-size: 15px;
  /* position: fixed; */
  bottom: 0;
  width: 100%;
}
.filterBox input {
  line-height: 50px;
  height: 13px;
  margin-top: 20px;
  width: 25px;
}
.filterBox label {
  margin-top: 20px;
  margin-right: 30px;
}
.container {
  background-color: rgb(192, 182, 168);
  width: 500px;
  border: 1px solid black;
}
</style>