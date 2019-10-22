<template>
    <div id="login">
        <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="80px" class="demo-ruleForm">
            <h3>
                <span> 请登录</span>
            </h3>
            <el-form-item label="用户名" prop="username">
                <el-input class="inp" v-model="ruleForm.username"></el-input>
            </el-form-item>

            <el-form-item label="密码" prop="password">
                <el-input class="inp"  type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
            </el-form-item>


            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                <el-button @click="registerForm('ruleForm')">注册</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
	import {mapMutations} from "vuex"

	export default {
		data() {
			return {
				ruleForm: {
					username: '',
					password: '',

				},
				rules: {
					username: [
						{required: true, message: '请输入用户名', trigger: 'blur'},
						{min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur'}
					],
					password: [
						{required: true, message: '请输入密码', trigger: 'blur'},
						{min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur'}
					],

				}
			};
		},
		methods: {
			submitForm(formName) {
				this.$refs[formName].validate((valid) => {
					if (valid) {
						console.log(this.ruleForm.username)
						// 把用户的信息传送到后台  向这个/Login接口传递数据
						this.$axios.post('/api/login', {
							username: this.ruleForm.username,
							password: this.ruleForm.password
						}).then(res => {
							if (res.data.errno) {  //如果登录失败  则弹出提示
								this.$message.error('该用户不存在,请注册');
								return
							}
							this.$message({
								message: '恭喜你,登录成功',
								type: 'success'
							});
							this.$router.push("/admin")

							// this.$store.state.loginUser = this.ruleForm.username
							// 要修改store (仓库)中的state数据   ,必须触发mutations中对应的函数 在函数内部修改state函数
							// this.$store.commit("saveLoginUser",this.ruleForm.username)
							this.saveLoginUser(this.ruleForm.username)
						})
						// alert('submit!');
					} else {
						console.log('error submit!!');
						return false;
					}
				});
			},
			...mapMutations(['saveLoginUser']),
			registerForm(formName) {

				this.$refs[formName].validate((valid) => {
					if (valid) {
						// 把用户的信息传送到后台  向这个/register传递数据
						this.$axios.post('/api/register', {
							username: this.ruleForm.username,
							password: this.ruleForm.password
						}).then(res => {
							if (res.data.errno) {  //如过失败, 则弹出失败提示
								this.$message.error('用户名已被抢先注册');
								return
							}
							this.$message({
								message: '注册成功',
								type: 'success'
							});
						})
					}
				})
			}
		}
	}
</script>

<style lang="less">

    @rem: 750/10rem;
    #login {
        position: fixed;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        height: 60%;
        padding: 0px 40/@rem 40/@rem 0px;
        margin: auto;
        /*box-shadow: 0 0 10px 0 pink;*/

    }



    #login .inp > input{
        min-height: 0!important;
        padding: 0 15/@rem;
        font-size: 34/@rem;
        border: 1px skyblue solid;
        border-radius: 20/@rem;
        height: 40px;
        line-height: 80/@rem;


    }

    #login h3 {
        font-size: 55/@rem;
        text-align: center;
        line-height: 70px;
    }

    .el-col {
        border-radius: 4px;
    }

    .bg-purple-dark {
        background: #99a9bf;
    }

    .bg-purple {
        background: #d3dce6;
    }

    .bg-purple-light {
        background: #e5e9f2;
    }

    .grid-content {
        border-radius: 4px;

    }

    .row-bg {
        padding: 10px 0;
        background-color: #f9fafc;
    }
</style>
