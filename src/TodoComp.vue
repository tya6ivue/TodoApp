<template>
  <div>
    <div class="container">
      <h2>My Vue Todo App</h2>
      <div class="input">
        <input type="text" v-model="task" placeholder="Enter task" class="" />
        <button class="button1" @click="submitTask">Add</button>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col">Delete</th>
            <th scope="col">Edit</th>
            <th scope="col">Submit</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>
              <!-- <input type="text" v-model="task.name" v-on:click="isHidden = false"> -->

              <!-- <button v-on:click="isHidden = true">Hide the text below</button> -->
              <!-- <button v-on:click="isHidden = !isHidden">Toggle hide and show</button>
  <h1 v-if="!isHidden">Hide me on click event!</h1> -->
              <!-- {{task}} -->
              <span
                :class="{ 'line-through': task.status === 'finished' }"
                v-if="!task.hidden"
              >
                {{ task.name }}
              </span>
              <h1 v-if="task.hidden">
                <input type="text" v-model="task.name" />
              </h1>
            </td>
            <td>
              <span
                class=""
                @click="changeStatus(index)"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-success': task.status === 'finished',
                  'text-warning': task.status === 'in-progress',
                }"
              >
                <!-- {{ capitalizeFirstChar(task.status) }} -->
              </span>
            </td>
            <td class="">
              <button class="button" @click="deleteTask(index)">
                <span class="">Delete</span>
              </button>
            </td>

            <button @click="editTask(index)" false class="button">Edit</button>

            <!-- <button  @click="editTask(index)"  class="button">Done</button> -->
            <!-- <h1 v-if="!isHidden">
           <input type="text" v-model="task.name" > </h1> -->

            <!-- <td class="">
            <button class="button" @click="editTask(index)">
              Edit
            </button>
          </td> -->
            <td>
              <input type="checkbox" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="checkbox">
      <label for="all">All:</label>
      <input
        type="checkbox"
        class="checkbox"
        id="all"
        value="all"
        @click="checkbox()"
      />

      <label for="Completed">Completed:</label>
      <input
        type="checkbox"
        id="Completed"
        value="Completed"
        class="checkbox"
        @click="checkbox()"
      />

      <label for="NotCompleted">Not Completed:</label>
      <input
        type="checkbox"
        id="notCompleted"
        value="notCompleted"
        @click="checkbox()"
        class="checkbox"
      />
    </div>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      hidden: true,
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Buy bananas from the supermarket.",
          status: "Pending",
        },
        {
          name: "Eat 1 kg chocolate in 1 hour.",
          status: "in-progress",
        },
        {
          name: "Create YouTube video.",
          status: "finished",
        },
      ],
    };
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.tasks[index].hidden = true;

      //   this.editedTask = index;
    },
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }
      this.task = "";
    },
    checkbox() {
      if (this.checkbox.checked == true) {
        this.checkbox.style.display = "block";
      } else {
        this.checkbox.style.display = "none";
      }
    },
  },
};
</script>
<style scoped>
.container {
  width: 550px;
  height: auto;
  border: 2px solid rebeccapurple;
}
.table {
}
table tr td {
  border: 1px solid rgb(171, 171, 224);
}
.Checkbox {
  width: 30px;
  font-size: 20px;
}
.button {
  width: 80px;
  color: aqua;
  background-color: blue;
  border: none;
  border-radius: 5px;
  height: 22px;
}
.button1 {
  margin-bottom: 10px;
  width: 80px;
  color: aqua;
  background-color: blue;
  border: none;
  border-radius: 5px;
  height: 22px;
}
.checkbox {
  width: 100%;
  display: flex;
  margin: 20px;
}
</style>