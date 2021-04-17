<template>
    <div>
        <el-container>
            <el-header>
                <img class="header-logo" src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimages.1111.com.tw%2Foad%2F9780424.jpg%3F2014%2F12%2F18%E4%B8%8B%E5%8D%8810%3A03%3A26&refer=http%3A%2F%2Fimages.1111.com.tw&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1621170064&t=1fd7ee4022e69d4471ba987e8f715b23" alt="">
            </el-header>
            <el-main>
                <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                    <el-form-item label="用户名" prop="username">
                        <el-input v-model="ruleForm.username"></el-input>
                    </el-form-item>
                    <el-form-item label="密码" prop="password">
                        <el-input type="password" v-model="ruleForm.password"></el-input>
                    </el-form-item>

                    <el-form-item>
                        <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                        <el-button @click="resetForm('ruleForm')">重置</el-button>
                    </el-form-item>
                </el-form>
            </el-main>
        </el-container>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data() {
            return {
                ruleForm: {
                    username: 'imok',
                    password: '111111'
                },
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' },
                        { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'change' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                        const _this = this
                        this.$axios.post('/login', this.ruleForm).then(res => {
                            console.log(res.headers)
                            console.log(res)
                            const jwt = res.headers['authorization']
                            const userInfo = res.data.data

                            // 把数据共享出去
                            _this.$store.commit("SET_TOKEN", jwt)
                            _this.$store.commit("SET_USERINFO", userInfo)

                            console.log(_this.$store.getters.getUser)

                            // 登录完成后跳转到blogs页面
                            _this.$router.push("/blogs")

                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>

<style scoped>
    .el-header, .el-footer {
        background-color: #B3C0D1;
        color: #333;
        text-align: center;
        line-height: 60px;
    }
    .el-main {
        /*background-color: #E9EEF3;*/
        color: #333;
        text-align: center;
        line-height: 160px;
    }
    .header-logo {
        height: 80%;
        margin-top: 6px;
        margin: 0 auto;
    }
    .demo-ruleForm {
        max-width: 500px;
        margin: 0 auto;
    }
</style>