<!-- 三要素：模板，js，style -->

<template>
    <div>

        <!-- 按钮 -->
        <el-button type="info" size="small" @click="toAddHandler">
            添加
        </el-button>
        <el-button type="danger" size="small">
            删除
        </el-button>
        <!-- 按钮 -->

        <!-- 表格 -->
        <el-table :data="customers">    
            <!-- 动态绑定数据 -->
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="telephone" label="联系方式"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="delet(slot.row.id)">删除</a>
                        <a href="" @click.prevent="toUpdata(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!-- 表格 -->

        <!-- 分页 -->
        <!-- <el-pagination layout="prev, pager, next" :total="200">
         </el-pagination> -->
        <!-- 分页 -->

        

        <!-- 对话框 -->
        <el-dialog title="录入顾客信息" :visible.sync="visible" width="60%" >
        
        <!-- 表单 -->
        <el-form    :model="form"   label-width="80px">
            <el-form-item   label="用户名">
                <!--    v-双向数据绑定   -->
                <el-input   v-model="form.username"></el-input>
            </el-form-item>
            <el-form-item   label="密码">
            <el-input   v-model="form.password" type="password"></el-input>
            </el-form-item>
            <el-form-item label="真实姓名">
            <el-input v-model="form.realname"></el-input>
            </el-form-item>
            <el-form-item label="手机号">
            <el-input v-model="form.telephone"></el-input>
            </el-form-item>
        </el-form>
        <!-- <el-button type="submit" @click="submit" value="提交"></el-button> -->
        <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="close">取 消</el-button>
            <el-button size="small" type="primary" @click="submit">确 定</el-button>
        </span>

        <!-- <span>这是一段信息</span>
        <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="close">取 消</el-button>     
            //@绑定事件
            <el-button size="small" type="primary" @click="close">确 定</el-button>
        </span> -->
        
        </el-dialog>
        <!-- 对话框 -->
    </div>

</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    methods:{//用于存放网页中需要的方法
        toAddHandler(){
            //
            this.form={
                type:'customer'
            }
            this.visible=true;
        },
        delet(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                //调用后台接口
                let url = "http://localhost:6677/customer/deleteById?id="+id;
                request.get(url).then((response)=>{
                    //1.刷新数据，2.提示结果，3.
                this.loadData();
                this.$message({
                                type: 'success',
                                message:response.message
                            });
                })

            
        })
        },
        toUpdata(row){
            //在模态框表单中显示当前行信息
            this.form=row;
            this.visible=true;
        },
        close(){
            this.visible = false;
        },
        submit(){//this.form 对象 --字符串-->后台
            //通过request与后台进行交互，并且携带参数

            let url = "http://localhost:6677/customer/saveOrUpdate"
            request.post(url,this.form);
            request({
                url,    //等价于 url:url
                method:"post",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form) //转换为查询字符串               
            }).then((response)=>{
                //请求结束，模态框关闭
                this.close();
                this.loadData();
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
        },
        loadData(){
            //vue实例创建完毕

        let url = "http://localhost:6677/customer/findAll"
        //可接受两个参数    response:四个属性
        request.get(url).then((response)=>{
                //将查询结果设置到customers
                this.customers=response.data;
        });
        }
    },
    data(){//用于存放网页中显示的数据
        return{
            visible:false,
            customers:[],
            form:{
                type:"customers"
            } //双向数据接收

        }
    },
    created(){
        this.loadData();
    }
    
}
</script>

<style scoped>
    /* .btn{
        background-color: darkcyan;
        color: #ffffff;
        display: inline-block;
        line-height: 2em;
        height: 2em;
        padding: 0 1em;
        cursor: pointer;
        border-radius: 3em;

    } */
</style>