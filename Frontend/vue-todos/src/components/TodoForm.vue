<template>
    <el-form @submit.prevent :inline="True" :model="formInput" class="todo-form-input">
        <el-form-item label="Title" prop="title">
            <el-input v-model="formInput.title" placeholder="Enter Todo">
            </el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit()">Add</el-button>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
import { ElMessage } from 'element-plus';
import { Options, Vue } from 'vue-class-component';
@Options({})
export default class TodoForm extends Vue{
    formInput = { title: "", completed: false };

    onSubmit(){
        if(this.formInput.title.length >3)
            this.$emit('send-message', this.formInput);
        else
            ElMessage({
                message: "Cuidado, Todo name é muito curto",
                type: "warning"
            })
        
    }
}
</script>