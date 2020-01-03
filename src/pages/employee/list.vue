<!-- 三要素：模板，js，style -->

<template>
    <div>
        <!-- 按钮 -->
        <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>
        <!-- /按钮 -->
     
        
        <!-- 表格 -->
        <el-table :data="employee">    
            <!-- //修改绑定数据 -->
            <!-- 动态绑定数据 -->
            <el-table-column prop="id" label="编号" fixed="left"></el-table-column>
            <el-table-column prop="realname" label="姓名" fixed="left"></el-table-column>
            <el-table-column width="150" prop="telephone" label="联系方式"></el-table-column>
            <el-table-column width="250" prop="idCard" label="身份证号"></el-table-column>
            <el-table-column width="250" prop="bankCard" label="银行卡号"></el-table-column>

            <el-table-column label="操作" fixed="right">
                <template v-slot="slot">    
                    <!-- //slot用于接收当前行信息    ${将脚本数据显示出来} -->
                    <!-- 
                         {{solt.row}}   //当前行
                         solt.row.id  //获取当前行的数据参数

                     -->
                    <a href="" @click.prevent="delet">删除</a>
                        <a href="" @click.prevent="updata">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!-- /表格 -->
       <!-- 分页 -->
        <el-pagination layout="prev, pager, next" :total="200">
         </el-pagination>
        <!-- 分页 -->

        <!-- 对话框 -->
        <el-dialog :title="title" :visible.sync="visible" width="60%" >
            --{{form}}
        <el-form label-width="80px" :model="form">
            <el-form-item label="用户名">
                <el-input v-model="form.username"/>
            </el-form-item>
            <el-form-item label="密码">
                <el-input type="password" v-model="form.password"/>
            </el-form-item>
            <el-form-item label="姓名">
                <el-input v-model="form.realname"/>
            </el-form-item>
            <el-form-item label="性别">
                <el-radio-group v-model="form.gender">
                    <el-radio label="男">男</el-radio>
                    <el-radio label="女">女</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="手机号">
                <el-input v-model="form.telephone"/>
            </el-form-item>
            <el-form-item label="身份证号">
                <el-input v-model="form.idCard"/>
            </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="close">取 消</el-button>
            <el-button size="small" type="primary" @click="submit">确 定</el-button>
        </span>
        </el-dialog>
        <!-- 对话框 -->
   </div>
</template>

<script>
import request from '@/utils/request'   //第三方库
import querystring from 'querystring'   //系统库
export default {
      created(){

          this.loadData();
      },
      methods:{//用于存放网页中需要的方法
        submit(){
            let url ="http://localhost:6677/waiter/saveOrUpdate "
            //前端向后台发送请求，完成数据的保存数据
            request({
                url,
                method:'post',
                //告诉后台我的请求体中放的是查询字符串
                Headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                //请求体中的数据,将this.form转换为查询字符串发送给后台
                data:querystring.stringify(this.form)
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
            let url="http://localhost:6677/waiter/findAll "
            request.get(url).then((response)=>{
                this.employee=response.data;
            })
        },
        toAddHandler(){
            this.title="添加员工信息",
            this.visible=true;
        },
        delet(id){
            // 不需要声明类型，因为是弱类型语言
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
        updata(){
            this.title="修改员工信息",
            this.visible=true;
        },
        close(){
            this.visible = false;
        }
    },
    data(){//用于存放网页中显示的数据
        return{
            visible:false,
            title:"录入员工信息",
            employee:[],
            form:{
                type:"waiter"
            }
        }
    }
}
</script>

<style scoped>

</style>