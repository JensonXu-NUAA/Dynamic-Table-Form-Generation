<template>
    <div class="AddNewItem">
        <el-button type="primary" plain @click="dialogVisible = true">添加新项目</el-button>
        <el-dialog title="添加新项目" v-model="dialogVisible" width="30%" :append-to-body="true">

            <div class="form">
                <el-form :model="form" label-width="auto" style="max-width: 600px">
                    <el-form-item v-for="(head, index) in tableHead" :key="index" :prop="'tableHead.' + index + '.inputValue'" :label="head.proporties_comment">
                        <el-input v-model="head.inputValue" style="width: 150%" placeholder="Please input" />
                    </el-form-item>
                </el-form>
            </div>

            <template #footer>
                <div class="dialog-footer">
                    <el-button type="primary" @click="submit">确认</el-button>
                    <el-button @click="dialogVisible = false">返回</el-button>
                </div>
            </template>
        </el-dialog>
    </div>
</template>

<script>
    import axios from "axios";

    export default{
        name: "AddNewItem",
        data() {
            return{
                dialogVisible: false,
                inputData: {}
            }
        },
        props: {
            tableHead:{
                type: Object,
                required: true
            },
            frontIndex: {
                type: Object,
                required: true
            }
        },
        methods: {
            // 提交表单数据
            async submit(){
                // 生成回传至后端的表单数据
                for(let i = 0; i < this.tableHead.length; i++){
                    let key = this.tableHead[i].proporties_name;
                    let value = this.tableHead[i].inputValue;
                    this.inputData[key] = value;
                }
                console.log(this.frontIndex);
                console.log(this.inputData);
                try{
                    const url = "http://127.0.0.1:8000/dialog/creation/";
                    const response = await axios.post(url, this.inputData);
                    console.log(response);
                    this.$emit('insert', this.inputData);  // 向父组件传递表单数据
                }catch(error){
                    console.error("Failed!");
                }
                this.dialogVisible = false;
            }
        }
    }
</script>

<style>
    .form{
        display: flex;
        justify-content: center
    }
</style>