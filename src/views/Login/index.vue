<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li :class="{'current': item.current}" v-for="item in menuTab" :key="item.id" @click="toggleMneu(item)">{{ item.txt }}</li>
            </ul>

           <!--表单 start-->
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form" size="medium">
                
                <el-form-item prop="username" class="item-from">
                    <label>邮箱</label>
                    <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>
                
                <el-form-item prop="password" class="item-from">
                    <label>密码</label>
                    <el-input type="text" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>
                
                <el-form-item prop="passwords" class="item-from" v-show="model === 'register'">
                    <label>重复密码</label>
                    <el-input type="text" v-model="ruleForm.passwords" autocomplete="off" minlength="6" maxlength="20"></el-input>
                </el-form-item>

                <el-form-item prop="code" class="item-from">
                    <label>验证码</label>
                    <el-row :gutter="10">
                        <el-col :span="15">
                            <el-input v-model.number="ruleForm.code"></el-input>
                        </el-col>
                        <el-col :span="9">
                            <el-button type="success" class="block" @click="getSms()">获取验证码</el-button>
                        </el-col>
                    </el-row>
                    
                </el-form-item>

                <el-form-item>
                    <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block" :disabled="anniu">{{ model==='login' ? "登录" : "注册"}}</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<script>
import sha1 from 'js-sha1';
import { GetSms } from "@/api/login";
import { reactive, ref, toRefs,isRef,onMounted } from '@vue/composition-api';
import { stripscript,validateEmail } from '@/utils/validate.js';
export default {
    name: 'login',
    setup(props, context){
        let validateCode = (rule, value, callback) => {
            let reg = /^[a-z0-9]{6}$/;
            if (value === '') {
                return callback(new Error('请输入验证码'));
            }
        };
        let validateUsername = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请输入用户名'));
            } else if(!validateEmail(value)){
                callback(new Error('用户名格式不规范！'));
            }else{
                callback();
            }
        };
        //验证重复密码的
        //if(model.value === 'login'){ callback(); }

        let validatePass = (rule, value, callback) => {
            let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/;

            ruleForm.password = stripscript(value)
            value=ruleForm.password

            if (value === '') {
                callback(new Error('请输入密码'));
            } else {
                callback();
            }
            // else if (value !== ruleForm.password) {
            //     callback(new Error('两次输入密码不一致!'));
            // } 
        };
        //这里放置data数据、生命周期、自定义的函数
        const menuTab = reactive([
            { txt: '登录',current: true,type: 'login' },
            { txt: '注册',current: false,type: 'register' }
        ])

        //模块值
        const model = ref('login')
        const anniu =ref('true')

        //model.value 取值
        //表单绑定数据
        const ruleForm = reactive({
            username: '',
            password: '',
            code: ''
        })
        //表单的验证
        const rules =reactive({
            username: [
                    { validator: validateUsername, trigger: 'blur' }
                ],
                password: [
                    { validator: validatePass, trigger: 'blur' }
                ],
                code: [
                    { validator: validateCode, trigger: 'blur' }
                ]
        })

        //判断是否是ref
        console.log(isRef(model) ? true : false)
        //const aa=toRefs(obj) 对象转成普通类型

        /**
         * 声明函数
         */
        const toggleMneu = (data => {
            menuTab.forEach(element => {
                element.current = false
            });
            data.current=true
            //修改模块值
            model.value = data.type
        })

        /**
         * 获取验证码
         */
        const getSms = (() => {
            GetSms({ username: ruleForm.username })
        })

        /**
         * 提交表单
         */
        const submitForm = (formName =>{

            // axios.request({
            //     method: 'post',
            //     url: '/user/12345',
            //     data: {
            //         firstName: 'Fred',
            //         lastName: 'Flintstone'
            //     }
            // })
            // .then(function (response) {
            //     console.log(response);
            // })
            // .catch(function (error) {
            //     console.log(error);
            // });
            
            context.refs[formName].validate((valid) => {
                if (valid) {
                    alert('submit!');
                } else {
                    console.log('error submit!!');
                    return false;
                }
            })
        })

        /**
         * 生命周期
         */
        //挂载完成后
        onMounted(() => {
            // console.log(process.env.VUE_APP_ABC)
        })
        return {
            menuTab,
            model,
            anniu,
            ruleForm,
            rules,
            toggleMneu,
            submitForm,
            getSms
        }
    },
}
</script>
<style lang="scss" scoped>
#login {
    height: 100vh;
    background-color: #344a5f;
}
.login-wrap {
    width: 330px;
    margin: auto;
}
.menu-tab {
    text-align: center;
    li {
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        color: #fff;
        cursor: pointer;
    }
    .current{
        background-color: rgba(0,0,0, .1);
    }
}
.login-form {
    margin-top: 29px;
    label {
        display: block;
        margin-bottom: 3px;
        font-size: 14px;
        color: #fff;
    }
}
.item-from {
    margin-bottom: 13px;
}
.block {
    display: block;
    width: 100%;
}
.login-btn {
    margin-top: 19px;
}
</style>