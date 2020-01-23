<template>
  <q-page class="bg-grey-3 column">
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
      tasks: [
        {
          title: "Get batanas",
          done: false
        },
        {
          title: "Eat batanas",
          done: false
        },
        {
          title: "Poo batanas",
          done: false
        }
      ]
    };
  },
  methods: {
    // deleteItem(idx) {
    //   this.tasks.splice(idx, 1);
    // },
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
          // console.log('>>>> OK')
        })
        .onOk(() => {
          // console.log('>>>> second OK catcher')
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
</style>
