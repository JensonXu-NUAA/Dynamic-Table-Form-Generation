<template>
    <div>
        <el-button type="primary" @click="getChange">按钮</el-button>
        <el-table v-if="tableVisible" :data="tableData" border style="width: 100%">
            <el-table-column v-for="(head, index) in tableHead" :key="index" :prop="head.proporties_name" :label="head.proporties_comment" />
            <el-table-column fixed="right" header-align="center" align="center" label="Operations" width="200px">
                <template #default="scope">
                    <AddNewItem :key="index" :tableHead="tableHead" :frontIndex="scope.$index" @insert="insertNewData(scope.$index, $event)"/>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<script>
    import { getTableInfo } from '@/api/tableInfo'
    import AddNewItem from '@/components/addNewItem.vue'

    export default{
        name: "tableView",
        data(){
            return {
                tableHead: null,  // 表头数据
                tableData: null,  // 表格数据
                tableVisible: false
            }
        },
        components: {
            AddNewItem
        },
        methods: {
            async getChange() {
                try {
                    const data = await getTableInfo('GET','action=list');
                    console.log(data);
                    // console.log(typeof data);
                    var JSONObj = JSON.parse(JSON.stringify(data))  // 转换成JSON对象
                    this.tableHead = JSONObj.proporties;  // 读取表头数据
                    this.tableData = JSONObj.content;  // 读取表格数据
                    this.tableVisible = true;
                    // this.tableData = JSON.stringify(data);
                    console.log(this.tableData);
                } 
                catch (error) {
                    console.error("An error occurred:", error);
                }
            },

            // 接收子组件传来的表单数据并在表格中插入行
            async insertNewData(index, json){
                this.tableData.splice(index + 1, 0, json);  // 在指定索引后插入新数据
            }
        }
    }
</script>
