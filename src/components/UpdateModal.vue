<template>
    <div>
        <q-dialog v-model="showModal" >
            <q-card style="width: 700px; max-width: 80vw;" class="q-pa-lg">
                <q-card-section>
                    <div class="text-h6">Update Todo List</div>
                </q-card-section>

                <q-card-section>
                    <q-form>
                        <q-input 
                            v-model="todo.userId"
                            square 
                            outlined 
                            label="User ID" 
                            class="q-mb-lg"
                        />
                        <q-input 
                            v-model="todo.title"
                            square 
                            outlined 
                            label="Title" 
                            class="q-mb-lg"
                            
                        />
                        <q-checkbox
                            v-model="todo.completed"
                            label="Is status complete?"
                        />
                        <div class="q-px-sm">
                            Status: <strong>'{{ todo.completed ? 'Completed' : 'Incomplete' }}'</strong>
                        </div>
                    </q-form>
                </q-card-section>

                <q-card-actions align="right" class="bg-white text-teal">
                    <q-btn color="teal" label="Modify" v-close-popup @click="updateForm()" />
                    <q-btn flat label="Close" v-close-popup />
                </q-card-actions>
            </q-card>
        </q-dialog>
    </div>
</template>

<script>
export default {
    props: {
        todo: Object,
    },
    data() {
        return {
            showModal: false,
        }
    },
    methods: {
        updateForm() {
            this.$axios
                .put(`https://jsonplaceholder.typicode.com/todos/${this.todo.id}`, this.todo)
                .then(response => {
                    alert('Successfully Updated!');
                });
        },
    },
}
</script>