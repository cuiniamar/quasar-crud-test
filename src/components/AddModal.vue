<template>
    <div>
        <q-dialog v-model="showModal">
            <q-card style="width: 700px; max-width: 80vw;" class="q-pa-lg">
                <q-card-section>
                    <div class="text-h6">Add Todo List</div>
                </q-card-section>

                <q-card-section>
                    <q-form>
                        <q-input 
                            v-model="forms.userId"
                            square 
                            outlined 
                            label="User ID" 
                            class="q-mb-lg"
                        />
                        <q-input 
                            v-model="forms.title"
                            square 
                            outlined 
                            label="Title" 
                            class="q-mb-lg"
                        />
                        <q-checkbox
                            v-model="forms.completed"
                            label="Is status complete?"
                        />
                        <div class="q-px-sm">
                            Status: <strong>'{{ forms.completed ? 'Completed' : 'Incomplete' }}'</strong>
                        </div>
                    </q-form>
                </q-card-section>

                <q-card-actions align="right" class="bg-white text-teal">
                    <q-btn color="teal" label="Create" v-close-popup @click="submitForm()" />
                    <q-btn flat label="Close" v-close-popup />
                </q-card-actions>
            </q-card>
        </q-dialog>
    </div>
</template>

<script>
let initialForms = {
    userId: null,
    title: null,
    completed: false
};

export default {
    data() {
        return {
            showModal: false,
            forms: Object.assign({}, initialForms)
        }
    },
    methods: {
        submitForm() {
            this.$axios
                .post('https://jsonplaceholder.typicode.com/todos', this.forms)
                .then(response => {
                    this.$emit("add-todo", response.data);
                    this.forms = Object.assign({}, initialForms);
                });
        },
    },
}
</script>