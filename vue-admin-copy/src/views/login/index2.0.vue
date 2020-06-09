<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="manu-tab">
                <li :class="{'current':item.current}" v-for="item in manuTab" :key="item.id" @click="toggleManu(item)">{{item.txt}}</li>
            </ul>
            <!--表单验证-->
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm"  class="login-form" style=" margin-top: 29px;" size="medium">
                <el-form-item  prop="username" class="item-form">
                    <label style="font-size: 14px;color: white;">邮箱</label>
                    <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item  prop="password" class="item-form">
                    <label style="font-size: 14px;color: white;">密码</label>
                    <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>

                <el-form-item  prop="passwords" class="item-form" v-if="model==='register'">
                    <label style="font-size: 14px;color: white;">确认密码</label>
                    <el-input type="password" v-model="ruleForm.passwords" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>

                <el-form-item  prop="code" class="item-form" style="margin-bottom: 17px;">
                    <label style="font-size: 14px;color: white;">验证码</label>
                    <el-row :gutter="15">
                        <el-col :span="15">
                            <el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input>
                            </el-col>
                        <el-col :span="8">
                             <el-button type="success" >获取验证码</el-button>
                        </el-col>
                    </el-row>   
                </el-form-item>

                <el-form-item>
                    <el-button type="danger" @click="submitForm('ruleForm')" class="block" style="width: 100%;
           display: block;margin-top: 15px">提交</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
import {stripscript, validateMail, validatePwd, validateVcode} from  '@/utils/validate'
    export default{
        name : "login",
        data (){
            //表单验证
            //验证验证码
            var checkCode = (rule, value, callback) => {
            if (value === '') {
            return callback(new Error('请输入验证码'));
            }
            else if( validateVcode(value)){
            callback(new Error('验证码格式有误'));
            }
            else{
            callback(); //true
            }
        };
        
        //验证用户名
        var validateUsername = (rule, value, callback) => {
            if (value === '') {
            callback(new Error('请输入用户名'));
            } else if(validateMail(value)){
            callback(new Error('用户名格式不正确'));
            }
            else{
            callback(); //true
            }
        };
        //验证密码
        var validatePassword = (rule, value, callback) => {
            this.ruleForm.password=stripscript(value)
            value=this.ruleForm.password
            if (value === '') {
            callback(new Error('请输入密码'));
            } else if (validatePwd(value)) {
            callback(new Error('密码为6至20位的数字+字母!'));
            } else {
            callback();
            }
        };
          //验证确认密码
        var validatePasswords = (rule, value, callback) => {
            this.ruleForm.passwords=stripscript(value)
            value=this.ruleForm.passwords
            if (value === '') {
            callback(new Error('请再次输入密码'));
            } else if (value !=  this.ruleForm.password) {
            callback(new Error('两次输入的密码不一致'));
            } else {
            callback();
            }
        };
            return{
                manuTab:[
                    {txt:"登陆",current:true,type:'login'},
                    {txt:"注册",current:false,type:'register'}
                ],
          //模块值
          model:'register',
          //表单验证
          ruleForm: {
          username: '',
          password: '',
          passwords: '',
          code: ''
        },
        rules: {
          username: [
            { validator: validateUsername, trigger: 'blur' }
          ],
          password: [
            { validator: validatePassword, trigger: 'blur' }
          ],
          passwords: [
            { validator: validatePasswords, trigger: 'blur' }
          ],
          code: [
            { validator: checkCode, trigger: 'blur' }
          ]
        }
            }
        },

        created(){},
        mounted(){},
        methods:{
            //数据驱动视图渲染
            toggleManu(data){
                //console.log(data)
                this.manuTab.forEach(element => {
                    element.current=false
                });
                data.current=true
                this.model=data.type
            },
        //表单相关方法
        submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
        },
    }
</script>

<style lang="scss" scoped>
    #login{
        height: 100vh;
        background-color: rgb(95, 90, 122);
    }
    .login-wrap{
        width: 330px;
        margin: auto;
    }
    .manu-tab{
        text-align: center;
        li{
            display: inline-block;
            width: 88px;
            line-height: 36px;
            font-size: 14px;
            color: white;
            border-radius: 2px;
            cursor: pointer;
        };
        .current{
            background-color: #444268;
        };
    .login-form{
        //margin-top: 29px;
        label{
            display: block;
            margin-bottom: 3px;
            font-size: 14px;
            color: white;
        }
    };  
    }
</style>