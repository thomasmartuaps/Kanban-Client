<template>
  <div>
    <!-- Navbar -->
    <nav class="border fixed split-nav navbar">
        <div class="nav-brand">
          <h3><b>KanChalk!</b></h3>
        </div>
        <div class="collapsible">
          <input id="collapsible1" type="checkbox" name="collapsible1">
          <button>
          <label for="collapsible1">
              <div class="bar1"></div>
              <div class="bar2"></div>
              <div class="bar3"></div>
            </label>
          </button>
          <div class="collapsible-body">
            <ul class="inline">
              <li>Welcome back, {{ email }}</li>
              <li class="align-middle"><img :src="avatar" width="35" height="35"></li>
              <li><button v-on:click="logout">Logout</button></li>
            </ul>
          </div>
        </div>
    </nav>
    <!-- End of navbar -->
    <div v-if="!isLoading" class="board">
      <div class="row flex-middle padding-left padding-right margin kanboard">
        <Column v-for="cat in categories" :key="cat.cat" :category="cat" :tasks="tasks"
        @add-task="addTask" @destroy="destroy" @modify="modify"></Column>
      </div>
    </div>
    <div v-else class="loading-screen">
      <h1>Loading ...</h1>
    </div>
  </div>
</template>

<script>
import Column from './Column.vue'

export default {
  name: 'Board',
  props: {
    email: String,
    avatar: String,
    tasks: Array,
    isLoading: Boolean
  },
  components: {
    Column: Column
  },
  data() {
    return {
      categories: [
        { cat: 'Backlog' },
        { cat: 'Ongoing' },
        { cat: 'Development' },
        { cat: 'Finished' },
      ]
    }
  },
  methods: {
    addTask: function (newItem) {
      this.$emit('create-task', newItem)
    },
    destroy: function (id) {
      this.$emit('destroy-item', id)
    },
    modify: function (item) {
      this.$emit('modify-item', item)
    },
    logout: function () {
      this.$emit('logout')
    }
  },
  computed: {
    profPic: function() {
      return `<img src="${this.avatar}" height="25" width="25">`
    }
  }
}
</script>

<style>
  .board {
    overflow: auto !important;
  }
  .loading-screen {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 95vh;
    width: 100vw;
    margin: auto;
    text-align: center;
  }
</style>