<template>
  <q-page class="bg-grey-3 column">

    <div class="row q-pa-sm bg-primary">
      <q-input v-model="newTask" placeholder="Add task" dense bg-color="white" class="col" square @keyup.enter="addTask">
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        clickable
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done}"
      >
        <q-item-section avatar>
          <q-checkbox class="no-pointer-events" v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn flat dense round color="primary" icon="delete" @click.stop="deleteTask(index)" />
        </q-item-section>
      </q-item>

    </q-list>

    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [
        {
          title: "Learn Quasar",
          done: false
        },
        {
          title: "Practise Quasar",
          done: false
        },
        {
          title: "Master Quasar",
          done: false
        },
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify('Task deleted')
      })
    },
    addTask(){
      if(this.newTask === '' || this.newTask === null){
        this.$q.notify({
        message: 'Please enter a new task',
        color: 'negative'
      })
      }
      else{
        this.tasks.push({
          title: this.newTask,
          done: false
        })
        this.newTask = ''
      }
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks{
  opacity: 0.5;
}
</style>
