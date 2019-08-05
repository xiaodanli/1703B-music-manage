<template>
    <el-form :model="login" status-icon :rules="rules" ref="login" label-width="100px" class="demo-ruleForm">
        <el-form-item label="用户名" prop="username">
            <el-input type="password" v-model="login.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
            <el-input type="password" v-model="login.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="loginFun">登录</el-button>
        </el-form-item>
    </el-form>
</template>
<script>
export default {
    data(){
        return {
            login:{
                username:'',
                password:''
            },
            rules: {
                username:[
                    { required: true, message: '用户名不能为空', trigger: 'blur'}
                ],
                password: [
                    { required: true,message:'密码不能为空', trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        loginFun(){
            this.$refs.login.validate((valid) => {
                if (valid) {
                    this.$api.user.login(this.login).then(res => {
                        if(res.data.code === 1){
                            this.$message({
                                message: '恭喜你，登录成功',
                                type: 'success'
                            });
                            window.localStorage.setItem('token',res.data.token);
                            this.$router.push('/music');
                        }else{
                            this.$message.error(res.data.message);
                        }
                    })
                } else {
                    console.log('error submit!!');
                    return false;
                }
            });
        }
    }
}
</script>

<style>
.demo-ruleForm{
    width:600px;
    margin:200px auto;
}
</style>
