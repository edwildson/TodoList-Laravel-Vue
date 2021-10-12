<template>
    <el-row>
        <el-col :span="12" :offset="7" style="width: 100%">
            <h1>TodoList - LARAVUE</h1>
            <TodoForm @send-message="createTodo"/>
            <el-table :data="todos">
                <el-table-column prop="title" label="Título" width="350" />
                <el-table-column fixed="right" label="Opções" width="200">
                    <template #default="scope">
                        <el-space wrap>
                            <el-switch 
                                v-model="scope.row.completed"
                                @click="updateTodo(scope.row)"
                            />
                            <el-popconfirm 
                            confirm-button-text="Sim"
                            cancel-button-text="Não"
                            icon="el-icon-info"
                            icon-color="red"
                            title="Você tem certeza que quer excluir?"
                            @confirm="deleteTodo(scope.row)"
                            @cancel="cancelDelete()">
                                <template #reference>
                                    <el-button size="mini"
                                    type="danger">
                                        Deletar
                                    </el-button>
                                        
                                    
                                </template>
                            </el-popconfirm>
                        </el-space>
                    </template>
                
                </el-table-column>

            </el-table>
        </el-col>
    </el-row>
</template>

<script lang="ts">
import {ElMessage} from 'element-plus';
import { Options, Vue } from 'vue-class-component';
import TodoForm from './TodoForm.vue';

interface Todo {
    title: string;
    completed: boolean;
    id: number;
}

@Options({
    components:{
        TodoForm,
    }
})
export default class TodoList extends Vue{
    todos = []

    
    async mounted(){
        await this.loadTodos();
    };

    async loadTodos(){
        const response = await this.axios.get(`http://localhost/api/todos`);
        this.todos = response.data
    };

    async createTodo(todo: Todo){
        console.log("Todo", todo);
        await this.axios.post(`http://localhost/api/todos`, {
            title: todo.title,
            completed: todo.completed
        });
        
        ElMessage({
            message: "Todo Created",
            type: "success"
        });

        await this.loadTodos();
    }


    async updateTodo(todo: Todo){
        console.log("Todo", todo);
        await this.axios.put(`http://localhost/api/todos/${todo.id}`, {
            title: todo.title,
            completed: todo.completed
        });
        
        ElMessage({
            message: "Todo Updated",
            type: "success"
        });

        await this.loadTodos();
    }

    async deleteTodo(todo: Todo){
        console.log("Todo", todo);
        await this.axios.delete(`http://localhost/api/todos/${todo.id}`);
        
        ElMessage({
            message: "Todo Deleted",
            type: "success"
        });

        await this.loadTodos();
    }

    cancelDelete(){
        console.log('Exclusão cancelada')
    };

    
    
    
}

</script>