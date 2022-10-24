<template>
  <div class="container" style="max-width: 2000px">
    <!-- Heading -->
    <h2 class="text-center mt-5">My ToDo List</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task.name"
        placeholder="Masukkan Tugas"
        class="w-100 form-control"
      />
      <input
        type="text"
        v-model="task.orang"
        placeholder="Masukkan Nama"
        class="w-100 form-control"
      />
      <button class="btn btn-primary rounded-0" @click="submitTask">
        SUBMIT
      </button>
    </div>
    <br>

    <!-- Task table -->
      <div class="card">
    <div class="card-header pb-0">
      <h6>List Tugas</h6>
    </div>
    <div class="card-body px-0 pt-0 pb-2">
      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
      <thead>
        <tr>
          <th scope="col" class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Tugas</th>
          <th scope="col" class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7 ">Nama</th>
          <th scope="col" class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Status</th>
          <th scope="col" class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Edit</th>
          <th scope="col" class="text-center text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <div class="d-flex px-2 py-1">
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
            </div>
          </td>
          <td>
            <div class="d-flex px-2 py-1">
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.orang }}
            </span></div>
          </td>
          <td class="text-center">
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'btn btn-danger ': task.status === 'to-do',
                'btn btn-success ': task.status === 'finished',
                'btn btn-warning ': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    </div>
    </div>
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
      task: {
        name: "",
        orang: "",
      },
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "Membuat Signin",
          status: "finished",
          orang: "Ana Putri"
        },
        {
          name: "Membuat Registrasi",
          status: "todo",
          orang: "Yusuf"
        },
        {
          name: "Membuat ToDo",
          status: "in-progress",
          orang: "Risya Aulia"
        },
        {
          name: "Mendeploy menggunakan Vercel",
          status: "to-do",
          orang: "Erico"
        },
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task.name = this.tasks[index].name;
      this.task.orang = this.tasks[index].orang;
      this.editedTask = index;
    },

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task.name;
        this.tasks[this.editedTask].orang = this.task.orang;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task.name,
          orang: this.task.orang,
          status: "todo",
        });
      }

      this.task.name = "";
      this.task.orang = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>