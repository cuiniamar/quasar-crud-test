<template>
  <div>
    <q-page class="flex flex-center q-px-xl">
      <q-table
        :data="todos"
        :columns="columns"
        :pagination="pagination"
        row-key="name"
        class="full-width"
      >
        <q-tr slot="header">
          <q-th colspan="3" class="text-left">
            <h2>Manage Todo List</h2>
          </q-th>
          <q-th>
            <q-btn icon="add" color="teal" @click="showAddModal()">Add Todo</q-btn>
          </q-th>
        </q-tr>
        <q-tr slot="body" slot-scope="props" :props="props">
          <q-td key="id" :props="props">{{ props.row.id }}</q-td>
          <q-td key="title" :props="props">
            {{ props.row.title }}
            <q-popup-edit v-model="props.row.title">
              <q-input v-model="props.row.title" dense autofocus counter />
            </q-popup-edit>
          </q-td>
          <q-td key="status" :props="props">
            <q-badge :color="props.row.completed ? 'teal' : 'negative'">
              {{ props.row.completed ? 'Completed' : 'Incomplete' }}
            </q-badge>
          </q-td>
          <q-td key="action" :props="props">
            <q-btn size="sm" round dense color="secondary" icon="edit" @click="showUpdateModal(props.row)" class="q-mr-sm"/>
            <q-btn size="sm" round dense color="primary" icon="delete" @click="showDeleteModal(props.rowIndex)"/>
          </q-td>
        </q-tr>
      </q-table>
    </q-page>

    <!-- add modal -->
    <add-modal
      ref="addModal"
      @add-todo="addTodo"
    ></add-modal>

    <!-- update modal -->
    <update-modal
      ref="updateModal"
      :todo="todoItem"
    ></update-modal>

    <!-- delete -->
    <q-dialog v-model="deleteModal" persistent>
      <q-card>
        <q-card-section class="row items-center">
          <span class="q-ml-sm">Are you sure you want to delete this Todo List?</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="Delete" color="negative" v-close-popup />
          <q-btn label="Cancel" color="teal" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import AddModal from 'components/AddModal.vue'
import UpdateModal from 'components/UpdateModal.vue'

export default {
  name: 'PageIndex',
  components: { 
    AddModal,
    UpdateModal,
  },
  data() {
    return {
      todos: [],
      deleteModal: false,
      todoItem: {},
      pagination: {
        sortBy: 'id',
        descending: true,
        rowsPerPage: 10,
      },
      columns: [
        { 
          name: 'id', 
          label: 'ID', 
          field: 'id',
          align: 'left'
        },
        {
          name: 'title', 
          label: 'Title', 
          field: 'title',
          align: 'left',
          field: row => row.title,
          format: val => `${val}`,
        },
        {
          name: 'status', 
          label: 'Status', 
          field: 'completed',
          align: 'center'
        },
        {
          name: 'action', 
          label: 'Action',
          field: "action",
          align: 'center'
        }
      ],
    }
  },
  mounted () {
    this.$axios
      .get('https://jsonplaceholder.typicode.com/todos')
      .then(response => {
        this.todos = response.data
      });
  },
  methods: {
    showAddModal() {
      this.$refs.addModal.showModal = true;
    },
    showUpdateModal(todo) {
      this.$refs.updateModal.showModal = true;
      this.todoItem = todo;
    },
    addTodo(todo) {
      this.todos.push(todo);
    },
    showDeleteModal(key) {
      this.todos.pop(key, 1);
    }
  },
}
</script>
