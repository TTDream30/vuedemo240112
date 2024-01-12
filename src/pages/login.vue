<template>
    <el-row class="login-container">
        <el-col :lg="16" :md="12" class="left">
            <div>
                <div class="first-div">欢迎光临</div>
                <div class="second-div"> 此站点是TT创建的用于测试代码编辑</div>
            </div>

        </el-col>
        <el-col :lg="8" :md="12" class="right">
            <h2 class="title">欢迎回来</h2>
            <div class="context">
                <span class="line"></span>
                <span>账号密码登录</span>
                <span class="line"></span>
            </div>
            <el-form ref="formRef" :rules="rules" :model="form" class="w-[250px]">
                <el-form-item prop="username">
                    <el-input v-model="form.username" placeholder="请输入用户名" >                        
                        <template #prefix>
                        <el-icon><User /></el-icon>
                        </template></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" v-model="form.password" placeholder="请输入密码" show-password>
                        <template #prefix>
                            <el-icon><Lock /></el-icon>
                        </template>
                    </el-input>
                </el-form-item>
                <el-form-item>
                    <el-button color="#626aef" round class="w-[250px]" type="primary" @click="onSubmit" :loading="loading">登 录</el-button>
                </el-form-item>

            </el-form>
        </el-col>
    </el-row>
</template>




<script setup>
import { ref,reactive,onMounted,onBeforeUnmount } from 'vue'
// import { login } from '~/api/manager'
import { ElNotification } from 'element-plus'

import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
// import { useCookies } from '@vueuse/integrations/useCookies'
import { setToken } from '~/composables/auth'
import { toast } from '~/composables/util'

const router = useRouter()
const store = useStore()
// do not use same name with ref
const form = reactive({
    username: '',
    password: '',
})

const rules = {
    username:[{ required: true, message: '用户名不能为空', trigger: 'blur' },
    { min: 1,  message: '用户名长度要在1-5个字符', trigger: 'blur' },],
    password:[{ required: true, message: '密码不能为空', trigger: 'blur' },
    { min: 1,  message: '密码长度要在1-5个字符', trigger: 'blur' },]
}

const formRef = ref(null)
const loading = ref(false)

const onSubmit = () => {
    formRef.value.validate((valid)=>{
        if(!valid){
            return false;
        }
        loading.value = true

        store.dispatch("login",form).then(res=>{
            toast('登录成功','success')
            // 跳转到后台首页
            router.push("/")
        }).finally(()=>{
            loading.value = false
        })
        // login(form.username,form.password).then(res=>{
        //     console.log("请求成功");
        //     console.log(res);

        //     toast('登录成功','success')

        // //     // 提示成功
        // //     ElNotification({
        // //     title: 'Success',
        // //     message: '登录成功',
        // //     type: 'success',
        // // })

        //     // 存储token和用户相关信息
        //     setToken(res.token)
        //     // getinfo().then(res2=>{
        //     //     store.commit("SET_USERINFO",res2)
        //     //     console.log(res2);
        //     // })

        //     // 跳转到后台首页
        //     router.push("/")
        

        // })
        // .finally(()=>{
        //     loading.value = false
        // })
    })
    console.log('submit!')
}

// 监听回车事件
function onKeyUp(e) {
    console.log(e);
    if (e.key == "Enter") {
        onSubmit()
    }
}

// 添加键盘监听
onMounted(()=>{
document.addEventListener("keyup",onKeyUp)
})

// 移除键盘监听
onBeforeUnmount(()=>{
    document.removeEventListener("keyup",onKeyUp)
})






</script>

<style scoped>
.login-container{
    @apply bg-indigo-500 min-h-screen;
}
.login-container .left ,.login-container .right{
    @apply flex items-center justify-center;
}
.login-container .right{
    @apply bg-light-50  flex-col;
}
.left .first-div{
    @apply font-bold text-5xl text-white mb-4;
}
.left .second-div{
    @apply text-gray-200 text-sm;
}
.right .title{
    @apply font-bold text-3xl text-gray-800;
}
.right .context{
    @apply flex items-center justify-center my-5 text-gray-300 space-x-2;
}
.context .line{
    @apply h-[1px] w-16 bg-gray-200;
}
</style>