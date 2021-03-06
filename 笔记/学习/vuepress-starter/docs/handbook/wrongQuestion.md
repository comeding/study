##### 下载文件请求

超连接下载传参，动态拼接参数

```js
 <a :href="'#/newsDetail/' + item.id ">
```





##### get请求文件流

a标签超链接下载

1.api封装设置responseType属性

```js
responseType:'blob'
```

2.请求接口回调书写如下代码

```js
let blob = new Blob([res.data], { type: "application/zip"});
          let url = window.URL.createObjectURL(blob);
          const link = document.createElement("a"); // 创建a标签
          link.href = url;
          link.download = "相关资料"; // 重命名文件
          link.click();
          URL.revokeObjectURL(url); // 释放内存
          this.checkList = [];
```

3.Blob 下载文件时 type 类型 大全

```js
.doc   application/msword
.docx  application/vnd.openxmlformats-officedocument.wordprocessingml.document

.xls   application/vnd.ms-excel
.xlsx  application/vnd.openxmlformats-officedocument.spreadsheetml.sheet

.pdf   application/pdf
.png   image/png
.jpg   image/jpg

.mp3   audio/mpeg

.zip   application/zip
```



##### el-upload上传文件解决跨域问题

1.设置el-upload执行前的方法： `:before-upload="beforeUpload"` 将文件转化为formdata数据上传(form上传没有跨域问题)

2.重写beforeUpload方法，请求上传接口

```js
beforeUpload(file){
      //将文件转换为formData
      let fd = new FormData()
      fd.append('file', file)
      //请求上传接口
      uploadFile(fd).then(response=>{})
}
```

3.:http-request    :auto-upload="true"

##### 嵌套路由报错：

Redirected when going from "/" to "/newproject/index" via a navigation guard（服务端添加路由）

##### 改变el-Table表格的行高属性

```js
:row-style="{height:'70px'}"
```

##### 前端实现分页页功能

```js
currentPageData:[], //当前页显示数据
totalPage: 1, // 统共页数，默认为1
currentPage: 1, //当前页数 ，默认为1
pageSize: 2, // 每页显示数量
productList:[ //所有数据
```

```js
     mounted(){

            // 计算一共有几页
                    this.totalPage = Math.ceil(this.productList.length / this.pageSize);
            // 计算得0时设置为1
                    this.totalPage = this.totalPage == 0 ? 1 : this.totalPage;
                    this.setCurrentPageData();
            },
```

```js
  methods:{
            // 设置当前页面数据，对数组操作的截取规则为[0~10],[10~20]...,
        setCurrentPageData() {
                let begin = (this.currentPage - 1) * this.pageSize;
                let end = this.currentPage * this.pageSize;
                this.currentPageData = this.productList.slice(begin,end)
                },
                            //上一页
            prevPage() {
                    console.log(this.currentPage);
                    if (this.currentPage == 1) return;

                    this.currentPage--;
                    this.setCurrentPageData();

            },
            // 下一页
            nextPage() {
                    if (this.currentPage == this.totalPage)return ;

                    this.currentPage++;
                    this.setCurrentPageData();

                    },
  }
```

##### element自定义表单校验--mixins混入

- el-form设置`:rules:'rules`

- el-form-item中设置prop，对应验证字段

- 在data对象中书写

  ```js
  rules:{
     name：{
         validator:validatePass,//自定义校验方法
         trigger: 'blur' //触发事件
     }
  }
  ```

- 新建mixins文件夹index.js文件,编写校验方法

  ```js
  export const validatePass = (rule,value,callback)=>{
      let reg =/^(13[0-9]|14[01456879]|15[0-35-9]|16[2567]|17[0-8]|18[0-9]|19[0-35-9])\d{8}$/;
      if(!value){
          return callback(new Error('不能为空'))
      }
      else if(!reg.test(value)){
          return callback(new Error('请输入电话号码'))
      }
  }
  ```

- 到需要使用的组件import引入即可（引入数组内，要使用引号！）

  ```js
  import { validatePass } from '@//mixins/index'
  export default{
      mixins:['validatePass']
  }
  ```

  

##### 图片上传跨域问题

设置`/mgr/bidder`拼接

##### el-table单选按钮

- @select-all="selectAll"

  ```js
  
      selectAll() {
        this.$refs.table.clearSelection();
      },
  ```

  

- @select="dialogCheck"

  ```js
              dialogCheck(selection, row){
               
                   this.$refs.table.clearSelection()
                    if (selection.length === 0) { // 判断selection是否有值存在
                        return
                    }
                    if (row) {
                        this.selectioned = row
                        this.$refs.table.toggleRowSelection(row, true)
                        //将值传到父组件
                        this.$emit('data',row.contractNum)
                    }
  
              }
  ```


##### 关于v-model父子组件传值失效的问题（大坑）

先更新数据，再打开弹窗，否则数据不能及时更新

```js
getById({
                id:row.id
              }).then(res=>{
                console.log(res);
                this.res = res.data.data
                this.display = true
              })
```

##### el-table分页

```js
:data="tableData.slice((currentPage-1)*pageSize,currentPage*pageSize)"

 currentPage:1,//当前页数
 pageSize:3//每页显示条数

 // 分页器
handleCurrentChange(val){
    this.currentPage = val
},
```

##### github无法上传问题

- 绑定远程仓库 

  ```js
  git remote add origin [git地址]
  ```

- push报错`unable to access ‘https://github.com/xxx/xxx.git/‘: OpenSSL SSL_read: Connection was reset, errno`

  执行两条命令

  ```js
  git config --global --unset http.proxy
  git config --global --unset https.proxy
  ```


##### el-input设置只读属性

```js
readonly
```

##### el-button禁用属性

```js
:disabled="true"
```

##### el-input字数限制属性

```js
maxlength="50"
```

##### download a标签下载属性,可直接设置下载文件名字

```js
download = "文件名"
```

##### 控制文件上传类型el-upload

- 设置属性accept

```js
accept=".pdf,.jpg,.jepg,.png"
accept=".xlsx,.xls,.pdf,.jpg,.jepg,.png"
```

##### 重写方法before-upload

```js
     //判断上传文件的格式
            beforeUpload(file){
               
                var testmsg=file.name.substring(file.name.lastIndexOf('.')+1)
                let xlsx = 'xlsx'
                let xls = 'xls'
                let pdf = 'pdf'
                let jpg = 'jpg'
                let png = 'png'
                if(testmsg!=xlsx&&testmsg!=xls&&testmsg!=pdf&&testmsg!=jpg&&testmsg!=png){
                    this.$message({
                        type:'error',
                        message:'只能上传xlxs,xls,pdf,jpg,png文件格式！'
                    })
                    return false
                }
            },
```

##### 限制文件上传大小

```js
   //限制文件大小
               const size = file.size / 1024 / 1024
                if (size > 4) {
                    this.$message({
                        type:'error',
                        message:'发票文件不得大于4M！'
                    })

                    return false
                }
```

##### 清除文件列表

```js
this.$refs.myUpload.clearFiles()
```

##### 控制文件列表显示

```js
:show-file-list="showFileList"
```

##### 在js文件中使用message

```js
import { Message } from 'element-ui';
Message.error('只能上传xlxs,xls,pdf,jpg,png文件格式！')
```

##### el-upload禁止输入

```js
slot="trigger"
slot="tip"
```

##### 禁止输入空格

```js
@keydown.native="keydown($event)"

 // 禁止输入空格
    keydown(e){
      if(e.keyCode == 32){
        e.returnValue = false
      }
    },
```

##### 向dialog传参生命周期的问题

利用watch监听参数变化

```js
 watch: {
            a: {
                handler(newVal, objVal) {
                        console.log(newVal);
                },
            }
```

##### el-date-picker时间误差8小时问题

设置value-format 属性, 精确到时间段

```js
value-format="yyyy-MM-dd HH:mm:ss"
```

##### 修改el-form-item的样式

```js
<el-form-item class="item">
    
 <style>
    .item .el-form-item__label{
        text-align:left;
    }
    </style>
```

##### el-upload上传多个文件

```js
:multiple="true"
```

##### el-upload上传新的文件覆盖之前上传的文件

```js
handleChange(res,file,filelist){
    if(filelist.length > 1){
        filelist.splice(0,1)
    }
}
```

##### el-table表格内容文本居中展示

```js
:cell-style="{'text-align':'center'}"
```

##### el-table表格表头文本居中展示

```js
:header-cell-style="{'text-align':'center'}"
```

##### 安装sass报错问题，正确步骤

1.使用npm安装node-sass 、sass-loader

```js
npm install --save-dev sass-loader
//sass-loader依赖于node-sass
npm install --save-dev node-sass
```

2.解决报错：`TypeError: this.getOptions is not a function`

sass-loader版本过高

```js
//删除原版本
npm uninstall sass-loader
//安装低版本
npm install sass-loader@6.0.0
```

##### 自适应布局`rem`+`vw`计算公式

设置时需要把`html`中`font-size`设置成`100px 对应的 vw值`的值

```js
假设屏幕 375px

计算公式 ：1vw = 3.75px 1px = 0.2666666666666667vw  100px = 26.66666666666667vw 
```

```css
html{
    font-size:26.66666666666667vw 
}

div{
    width:5rem
    height:0.5rem
        ···
}
```

媒体查询超出宽度范围后固定`body`宽度，内容居中

```css
@media screen and (min-width: 768px) {
  html {
    max-width: 768px;
    font-size: 9.765625vw
  }
}
```

##### el-select多选

```js
multiple属性
```

##### 日期时间格式转换

```js
npm install -save-dev moment
import moment from 'moment'
moment(date).format("YYYY-MM-DD")
```

##### vue禁止复制粘贴

```js
created(){
    this.$nextTick(() => {
                // 禁用右键
                document.oncontextmenu = new Function("event.returnValue=false");
                // 禁用选择
                document.onselectstart = new Function("event.returnValue=false");
            });
  },
```

##### el-dialog嵌套弹出框，被遮罩层覆盖的问题

```js
append-to-body 属性
```

##### 关于图片报错：ElImage Invalid prop: type check failed for prop "src". Expected String, got Array 

类型转换

```js
:src="String(row.contractFile)"
```

##### el-input回车事件

```js
<el-input  v-model="input" @keyup.enter.native="search1">
```

##### el-input只能输入数字

```js
onkeyup="value=value.replace(/[^\d]/g,'')"
```

##### 拖拽排序

vuedraggable

```js
https://www.itxst.com/vue-draggable/tutorial.html
```

##### 生成15位随机数

```js
function rando(m) {
            var num = '';
            for (var i = 0; i < m; i++) {
                var val = parseInt(Math.random() * 10, 10);
                if (i === 0 && val === 0) {
                    i--;
                    continue;
                }
                num += val;
            }
            return num;
        }
        console.log(rando(15));
```

##### 自适应布局：postcss-px-to-viewport

在项目根目录下添加.postcssrc.js文件

```js

//添加如下配置：
module.exports = {
    plugins: {
      autoprefixer: {}, // 用来给不同的浏览器自动添加相应前缀，如-webkit-，-moz-等等
      "postcss-px-to-viewport": {
        unitToConvert: "px", // 要转化的单位
        viewportWidth: 1920, // UI设计稿的宽度
        unitPrecision: 6, // 转换后的精度，即小数点位数
        propList: ["*"], // 指定转换的css属性的单位，*代表全部css属性的单位都进行转换
        viewportUnit: "vw", // 指定需要转换成的视窗单位，默认vw
        fontViewportUnit: "vw", // 指定字体需要转换成的视窗单位，默认vw
        selectorBlackList: ["wrap"], // 指定不转换为视窗单位的类名，
        minPixelValue: 1, // 默认值1，小于或等于1px则不进行转换
        mediaQuery: true, // 是否在媒体查询的css代码中也进行转换，默认false
        replace: true, // 是否转换后直接更换属性值
        exclude: [/node_modules/], // 设置忽略文件，用正则做目录名匹配
        landscape: false // 是否处理横屏情况
      }
    }
  };
  
```

##### 滚动条

```css
<el-scrollbar style="height: 700px">
       <div></div>
</el-scrollbar>

<style lang="scss" scoped>
/deep/.el-scrollbar__wrap {
  overflow-x: hidden;
}
</style>
```

