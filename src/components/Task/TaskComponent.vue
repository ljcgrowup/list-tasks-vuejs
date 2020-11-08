<template>
    <div>
        <h2>{{ title }}</h2>
        
        <div class="row">
            <div class="col">
                <form @submit.prevent="onSubmmit">
                    <div class="form-inline">
                        <input type="text" placeholder="Nova tarefa" class="form-control" v-model="name">
                        <button class="btn btn-primary">Adicionar Tarefa</button>
                    </div>            
                </form>
            </div>
            <div class="col">
                <form>
                    <input type="text" placeholder="Pesquisar" class="form-control" v-model="filter">
                </form>
            </div>
        </div>

        <table class="table table-dark table-hover">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Nome</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in filteredTasks" :key="index">
                    <td>{{ task.id }}</td>
                    <td>{{ task.name }}</td>
                    <td>
                        <a
                        href="#" 
                        style="margin-right: 5px;" 
                        class="btn btn-info"
                        @click.prevent="edit(task.id)"
                        >Editar</a>
                        <a href="#" class="btn btn-danger" @click.prevent="destroy(task.id)">Excluir</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
  data() {
    return {
      title: "Lista de Tarefas",
      tasks: [],
      task: {
          id: '',
          name: '',
      },
      updating: false,
      updateIndex: '',
      name: '',
      filter: ''
    }
  },
  methods: {
      onSubmmit() {
          if (this.updating) {
              this.update()
              return
          }

          this.save()
      },
      save() {
          this.task.id = this.tasks.length + 1
          this.task.name = this.name
          this.tasks.push(this.task)
          this.clearTask()
      },
      edit(id) {
        let index = this.findTask(id)
        this.task = this.tasks[index]
        this.name = this.task.name
        this.updateIndex = index
        this.updating = true
      },
      findTask(id) {
          for (let index = 0; index <= this.tasks.length; index++) {
              if (this.tasks[index].id == id)
                return index
          }
      },
      update() {
          this.task.name = this.name
          this.tasks[this.updateIndex] = this.task
          this.clearTask()
          this.updating = false
      },
      destroy(id) {
          this.tasks.splice(this.findTask(id), 1)
      },
      clearTask() {
          this.task = {
              id: '',
              name: ''
          }
          this.name = ''
      }
  },
  computed: {
      filteredTasks() {
          if (this.filter === '') {
              return this.tasks
          }

          let vm = this
          return this.tasks.filter(task => {
              return task['name'].includes(vm.filter)
          })
      }
  }
}
</script>

<style scoped>
form {
    margin: 20px 0;
}
</style>