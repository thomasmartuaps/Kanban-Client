<template>
        <div class="card padding-none align-middle cat">
          <div class="card-header">{{ category.cat }}</div>
            <div class="card-body column-bod">
              <Card v-for="task in selector" :key="task.id" :task="task"
              @destroy="destroy" @modify="modify"></Card>
            </div>
          <div class="card-footer">
            <form v-if="summonFormAdd" class="form-group" id="add-task">
                <p> <b>Add Task:</b> </p>
                <input type="text" id="title" v-model="title" placeholder="Title"><br>
                <input type="text" id="description" v-model="description" placeholder="Description"><br>
                <button v-on:click.prevent="addTask">Add</button>
                <button v-on:click.prevent="hideForm">close</button>
            </form>
            <button class="margin-none" v-on:click="summonForm">Add Task</button></div>
        </div>
</template>

<script>
import Card from './Card.vue'
import AddForm from './AddForm.vue'

export default {
  name: 'Column',
  props: {
      category: Object,
      tasks: Array
  },
  components: {
    Card: Card
  },
  data() {
    return {
      title: '',
      description: '',
      summonFormAdd: false
    }
  },
  methods: {
    summonForm: function () {
      this.summonFormAdd = true
      this.title = ''
      this.description = ''
    },
    hideForm: function () {
      this.summonFormAdd = false
    },
    addTask: function () {
      this.$emit('add-task', {
          title: this.title,
          description: this.description,
          category: this.category.cat
      })
      this.hideForm()
    },
    destroy: function (id) {
        this.$emit('destroy', id)
    },
    modify: function (item) {
        console.log('edit sampai column')
        this.$emit('modify', item)
    }
  },
  computed: {
    selector: function () {
      return this.tasks.filter(el=> el.category === this.category.cat)
    }
  }

}
</script>

<style>
  .cat {
    margin: 5px;
    margin-top: 10%;
    width: 100% !important;
    min-height: 15cm;
  }
  @media only screen and (min-width: 600px) {
    .cat {
      margin-top: 7%;
      width: 100% !important;
    }
    .column-bod {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  }
  @media only screen and (min-width: 900px) {
    .cat {
      margin-top: 5%;
      width: 45% !important;
    }
    .column-bod {
      display: inline;
    }
  }
  @media only screen and (min-width: 1280px) {
    .cat {
      margin-top: 2%;
      width: 9cm !important;
    }
  }
</style>