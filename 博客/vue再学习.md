# vue再学习

## vue项目搭建

1.创建Vue工程
npm init vue@latest
2.安装依赖
Element-Plus

npm install element-plus --save

Axios
npm install axios
Sass
npm install sass -D

修改main.js配置文件



3.目录调整
删除components下面自动生成的内容

（src下）新建目录api、utils（将request.js复制到下面《大事件前端H:\06_前端.rar\06_前端\12_大事件资料\02_请求工具request.js》资料、views

将资料中的静态资源拷贝到assets目录下《H:\06_前端.rar\06_前端\12_大事件资料\01_静态资源assets》

删除App.uve中自动生成的内容



### 定义参数、方法、数据模型

数据模型的定义

```
//定义数据模型

const registerData = ref({
  username: '',
  password: '',
  rePassword: '',

})
```



表单校验规则的定义

```
const rules = ref({
    username: [
        { required: true, message: '请输入用户名', trigger: 'blur' },
        { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' },
    ],
    password: [
        { required: true, message: '请输入密码', trigger: 'blur' },
        {min: 6, max: 20, message: '长度在 6 到 20 个字符', trigger: 'blur' },
    ],
    rePassword: [
        { required: true, message: '请再次输入密码', trigger: 'blur' },
        {min: 6, max: 20, message: '长度在 6 到 20 个字符', trigger: 'blur' },
        { validator: (rule, value, callback) => {
            if (value!== registerData.value.password) {
                callback(new Error('两次输入密码不一致'))
            } else {
                callback()
            }
            }
            , trigger: 'blur'
        },
    ],
})
```

```
required：（true/false）规定一个文本区域是必需的/必须填写
trigger：	blur失去焦点；	change数据改变。
```

```
表单数据项 挂载 	:model="registerData"
校验规则 挂载		 :rules="rules"

<el-form ref="form" size="large" autocomplete="off" v-if="isRegister" :model="registerData" :rules="rules"> 
```



### 前后端跨域问题

由于浏览器的同源策略限制，向不同源(不同协议、不同域名、不同端口)发送ajax请求会失败

![image-20240603195913560](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20240603195913560.png)
