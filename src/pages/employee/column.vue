<template>
    <div>
        栏目管理<br>
        <el-button size="small" type="primary" @click="toAddHanler"> 添加</el-button>
    <el-button size="small" type="danger"> 删除</el-button>
<el-table :data="column">
    <el-table-column label="编号" prop="id"></el-table-column>
    <el-table-column label="栏目名称" prop="name"></el-table-column>
    <el-table-column label="序号" prop="num"></el-table-column>
    <el-table-column label="父栏目" prop="parentId"></el-table-column>
    <el-table-column label="操作">
        <template v-slot="slot">  
            <a href="" @click.prevent="toUpdateHandler" class = "el-icon-edit"></a>
            <a href="" @click.prevent="toDeleteHandler(slot.row.id)" class="el-icon-delete"></a>
            <!-- <a href="" @click.prevent="toDetailsHandler" class="el-icon-more"></a> -->
        </template>
    </el-table-column>
</el-table>
        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <el-dialog :title="title" :visible.sync="visiable" width="60%">
             ---{{form}}
            <el-form :model="form">
                <el-form-item label="栏目名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="序号">
                    <el-input v-model="form.num"></el-input>
                    </el-form-item>
            </el-form>
             <span slot="footer" class="dialog-footer">
                <el-button size="small" @click="closeModelHandler">取 消</el-button>
                <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>
<script>
import querystring from 'querystring'
import request from'@/utils/request'

export default {
    data(){
        return{
            
            form:{type:""},
            
            title:"录入栏目信息",
            visiable:false,
         //   dropmenu:[],
            column:[],
               // options:[],
            
        }
        
    },
    methods:{
        // dropmenuloadData(){
        // let url = "http://localhost:6677//category/findAll"
        //  request.get(url).then((response)=>{
        // // 将查询结果设置到customers中，this指向外部函数的this
        // this.dropmenu = response.data; 
        // }) 
        // },
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        }).then(() => {
            let url="http://localhost:6677/category/deleteById?id="+id;
                request.get(url).then((response)=>{
                    this.loadData();
          this.$message({
            type: 'success',
            message: response.message
          });
        })
        })
        },
        toUpdateHandler(){
           this.title = "修改栏目信息";
           this.visiable = true;
        },
        // toDetailsHandler(){
        //    this.title = "栏目详情";
        //    this.visiable = true;
        // },
        toAddHanler(row){
                this.title = "录入栏目信息",
                this.visiable = true;
        },
        closeModelHandler(){
                this.visiable = false;
        },
        submitHandler(){
                 let url = "http://localhost:6677/category/saveOrUpdate";
            request({
                url,
                method:"POST",
                headers:{
                         "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeModelHandler();
                this.loadData();
                this.$message({
                    type:"succsee",
                    message:response.message
                })
            })
        },
        loadData(){
        let url = "http://localhost:6677/category/findAll"
         request.get(url).then((response)=>{
        // 将查询结果设置到customers中，this指向外部函数的this
        this.column = response.data;
      })
        } 

    },
    created(){
        this.loadData()
        
    },
    
}
</script>
<style scoped>

</style>