<!-- 三要素：模板，js，style -->

<template>
    <div>
        <!-- 按钮 -->
        <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">批量删除</el-button>
        <!-- /按钮 -->
     
        
        <el-table :data="product">    
            <!-- //修改绑定数据 -->
            <!-- 动态绑定数据 -->
            <el-checkbox-group>
            <el-table-column  fixed="left" type="selection" >
                
            </el-table-column>
            </el-checkbox-group>
            <el-table-column  prop="id" label="编号" fixed="left">

            </el-table-column>
            <el-table-column prop="name" label="产品名称" fixed="left">

            </el-table-column>
            <el-table-column  prop="categoryId" label="所属产品">
                
            </el-table-column>
            <el-table-column  prop="price" label="价格">

            </el-table-column>
            <el-table-column  prop="description" label="描述">

            </el-table-column>
            


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
    
        <!-- 对话框 -->
        <el-dialog :title="title" :visible.sync="visible" width="60%" >
            --{{form}}
        <el-form label-width="80px" :model="form">
            <el-form-item label="名称">
                <el-input v-model="form.name"/>
            </el-form-item>
            <el-form-item label="价格">
                <el-input  v-model="form.price"/>
            </el-form-item>
            <el-form-item label="所属栏目">
                <el-select v-model="value" placeholder="请选择">
                    <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="介绍">
                <el-input v-model="form.description"/>
            </el-form-item>
            <el-form-item label="产品主图">
                <el-button >点击上传</el-button>
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
        check(){
            ischeak=true
        },
        submit(){
            let url ="http://localhost:6677/product/saveOrUpdate "
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
            let url="http://localhost:6677/product/findAll  "
            request.get(url).then((response)=>{
                this.product=response.data;
            })
        },
        toAddHandler(){
            this.title="添加产品信息",
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
            title:"添加产品信息",
            ischeak:false,
            product:[],
            form:{},
            options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }]
        }
    }
}
</script>

<style scoped>

</style>