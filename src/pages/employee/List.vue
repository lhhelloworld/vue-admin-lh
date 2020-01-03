<template>
<div>
    <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
    <el-button type="danger" size="small">批量删除</el-button>
    <el-table :data="employees">
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="realname" label="姓名"></el-table-column>
        <el-table-column prop="gender" label="性别"></el-table-column>
        <el-table-column width="120" prop="telephone" label="手机号"></el-table-column>
        <el-table-column width="200" prop="idCard" label="身份证号"></el-table-column>
        <el-table-column width="200" prop="gender" label="银行卡号"></el-table-column>
        <el-table-column prop="username" label="用户名"></el-table-column>
        <el-table-column prop="操作">
            <template v-slot="slot">
                 <a href="" @click.prevent="toDeleteHandler(slot.row.id)"><i class="el-icon-delete"></i></a>
                 <a href="" @click.prevent="toUpdateHandler(slot.row)"><i class="el-icon-edit"></i></a>
                  <a href="">详情</a>
            </template>
        </el-table-column>
    </el-table>
    <el-pagination
    layout="prev, pager, next" :total="50">
    </el-pagination>
    <el-dialog
        title="录入员工信息"
        :visible.sync="visible"
        width="60%">
         <el-form :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.username"></el-input>
            </el-form-item>
             <el-form-item label="密码">
                <el-input type="password" v-model="form.password"></el-input>
            </el-form-item>
             <el-form-item label="真实姓名">
                <el-input v-model="form.realname"></el-input>
            </el-form-item>
             <el-form-item label="手机号">
                <el-input v-model="form.telephone"></el-input>
            </el-form-item>
             <el-form-item label="性别">
                <el-input v-model="form.gender"></el-input>
                    <el-radio label="男">男</el-radio>
                    <el-radio label="女">女</el-radio>
            </el-form-item>
             <el-form-item label="身份证号">
                <el-input v-model="form.idCard"></el-input>
            </el-form-item>
             <el-form-item label="银行卡号">
                <el-input v-model="form.backCard"></el-input>
            </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
        </span>
    </el-dialog>
    </div>    
    </template>
    <script>
    import request from '@/utils/request'
    import querystring from 'querystring'
    export default { 
        methods:{
            loadData(){
                let url ="http://localhost:6677/waiter/findAll"
                request.get(url).then((response)=>{
                // 将查询结果设置到customers中，this指向外部函数的this
                this.employees = response.data;            
            })
            },

            toDeleteHandler(id){
                this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        }).then(() => {
            let url ="http://localhost:6677/waiter/deleteById?id="+id;
            request.get(url).then((response)=>{
                this.loadData();
                this.$message({
                    type: 'success',
                    message: '删除成功!'
            }); 
            })
        
        })
                },
            toUpdateHandler(row){
                this.form = row;
                this.visible =true;
                },
            closeModalHandler(){
                this.visible =false;
                },
            toAddHandler(){
                this.form = {
                 type :"customer"  
                }
                this.visible =true;
                },
            submitHandler(){
                let url ="http://localhost:6677/waiter/saveOrUpdate "
                request({
                    url,
                    method:"POST",
                    headers:{
                        "Content-Type":"application/x-www-form-urlencoded"
                    },
                    data:querystring.stringify(this.form)
                }).then((response)=>{
                    this.closeModalHandler()
                    this.loadData()
                    this.$message({
                        type:"success",
                        message:response.message
                    })
                
            })
            }
        },   
        data(){
            return{
                visible:false,
                employees:[],
                form:{
                    type:"waiter"
                }
                }
        },
        created(){
           this.loadData()
        }
    }
    </script>
    <style scoped>
    </style>