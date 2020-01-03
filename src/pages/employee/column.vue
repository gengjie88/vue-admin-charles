<template>
    <div>
        产品管理<br>
        <el-button size="small" type="primary" @click="toAddHanler"> 添加</el-button>
    <el-button size="small" type="danger"> 删除</el-button>
<el-table :data="column">
    <el-table-column label="编号" prop="id"></el-table-column>
    <el-table-column label="产品名称" prop="name"></el-table-column>
    <el-table-column label="价格" prop="price"></el-table-column>
    <el-table-column label="描述" prop="description"></el-table-column>
    <el-table-column label="所属产品" prop="categoryId"></el-table-column>
    <el-table-column label="操作">
        <template v-slot="slot">
            <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
            <a href="" @click.prevent="toUpdateHandler">修改</a>
            <a href="" @click.prevent="toDetailsHandler">详情</a>
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
                <el-form-item label="产品名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
            <el-form-item label="选择所属类型">
                 <el-dropdown split-button type="primary" @click="handleClick" >
                请选择
            <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>喜羊羊</el-dropdown-item>
            <el-dropdown-item>美羊羊</el-dropdown-item>
            <el-dropdown-item>懒羊羊</el-dropdown-item>
            <el-dropdown-item>沸羊羊</el-dropdown-item>
            <el-dropdown-item>暖羊羊</el-dropdown-item>
            <el-dropdown-item>慢羊羊</el-dropdown-item>
        </el-dropdown-menu>
    </el-dropdown>
            </el-form-item>
             
                <el-form-item label="价格">
                    <el-input v-model="form.price"></el-input>
                </el-form-item>
                <el-form-item label="描述">
                    <el-input v-model="form.description"></el-input>
                </el-form-item>
                <!-- <el-form-item label="所属产品">
                    <el-input v-model="form.categoryId"></el-input>
                </el-form-item> -->

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
            form:{type:"column"},
            title:"录入产品信息",
            visiable:false,
            column:[]
        }
        
    },
    methods:{
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })
        },
        toUpdateHandler(){
           this.title = "修改产品信息";
           this.visiable = true;
        },
        toDetailsHandler(){
           this.title = "产品详情";
           this.visiable = true;
        },
        toAddHanler(row){
                this.title = "录入产品信息",
                this.visiable = true;
        },
        closeModelHandler(){
                this.visiable = false;
        },
        submitHandler(){
                 let url = "http://localhost:6677//product/saveOrUpdate";
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
        let url = "http://localhost:6677/product/findAll"
         request.get(url).then((response)=>{
        // 将查询结果设置到customers中，this指向外部函数的this
        this.column = response.data;
      })
        },

    },
    created(){
        this.loadData()
    },
    
}
</script>
<style scoped>

</style>