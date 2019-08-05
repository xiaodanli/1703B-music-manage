<template>
    <el-form :model="registry" status-icon :rules="rules" ref="registry" label-width="100px" class="demo-ruleForm">
        <el-form-item label="用户名" prop="username">
            <el-input type="text" v-model="registry.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="pass">
            <el-input type="password" v-model="registry.pass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPass">
            <el-input type="password" v-model="registry.checkPass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="registryFun">注册</el-button>
        </el-form-item>
    </el-form>
</template>
<script>
export default {
    data(){
        var validatePass = (rule, value, callback) => {
            if (value === '') {
            callback(new Error('请输入密码'));
            } else {
            if (this.registry.checkPass !== '') {
                this.$refs.registry.validateField('checkPass');
            }
            callback();
            }
        };
        var validatePass2 = (rule, value, callback) => {
            if (value === '') {
            callback(new Error('请再次输入密码'));
            } else if (value !== this.registry.pass) {
            callback(new Error('两次输入密码不一致!'));
            } else {
            callback();
            }
        };
        return {
            registry:{
                username:'',
                pass:'',
                checkPass:''
            },
            rules: {
                username:[
                    { required: true, message: '用户名不能为空'}
                ],
                pass: [
                    { required: true,validator: validatePass, trigger: 'blur' }
                ],
                checkPass: [
                    { required: true,validator: validatePass2, trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        registryFun(){
            this.$refs.registry.validate((valid) => {
                if (valid) {
                    this.$api.user.registry({username:this.registry.username,password:this.registry.pass}).then(res => {
                        if(res.data.code === 1){
                            this.$message({
                                message: '恭喜你，注册成功',
                                type: 'success'
                            });
                            this.$router.push('/login');
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
    margin:0 auto;
}
</style>
