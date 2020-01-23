<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTaskTitle"
        @keyup.enter="addTask()"
        class="col"
        bg-color="white"
        square
        filled
        placeholder="Add task"
        maxlength="12"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click.stop="addTask()" />
        </template>
      </q-input>
      <div v-if="!tasks.length" class="no-tasks absolute-center">
        <q-icon name="check" size="100px" color="primary" />
        <div color="primary" class="text-h5 text-primary text-center">No tasks</div>
      </div>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, idx) in tasks"
        :key="idx"
        tag="label"
        :class="{'task-done bg-blue-1': task.done}"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primary"
            dense
            @click.stop="confirmDeleteItem(idx)"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTaskTitle: ""
    };
  },
  methods: {
    addTask() {
      this.tasks.push({ done: false, title: this.newTaskTitle });
      this.newTaskTitle = "";
      this.$q.notify({
        color: "secondary",
        message: "Task added",
        position: "top"
      });
    },
    confirmDeleteItem(idx) {
      this.$q
        .dialog({
          title: "Подтверждение",
          message: "Реально удалить?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(idx, 1);
        })
        .onOk(() => {
          this.$q.notify({
            message: "Task deleted",
            color: "secondary",
            position: "top"
          });
        })
        .onCancel(() => {
          // console.log('>>>> Cancel')
        })
        .onDismiss(() => {
          // console.log('I am triggered on both OK and Cancel')
        });
    }
  }
};
</script>

<style lang="scss">
.task-done {
  .q-item__label {
    text-decoration: line-through;
    color: gray;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
