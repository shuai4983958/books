
### gitBook接入文档步骤

####  环境配置
> 1. gitbook命令行工具
```
npm install gitbook-cli -g
```
> 2. gitbook命令 初始化项目
```
gitbook init
```
> 3. 本地预览书籍 （http://localhost:4000/）
```
gitbook serve
```
> 4. 打包生成静态资源
```
gitbook build
该命令会在当前文件夹中生成_book文件夹，用户可以将这个文件夹内容托管到网上，从而实现内容的发布。
```

#### 目录结构
```

├── book.json   // 配置文件
├── pages       // gitbook资源文件根目录
|   ├── README.md    // 前言或简介
|   └── SUMMARY.md   // gitbook目录
|   └── aijianzi/   
|   └────── README.md
|   └────── something.md
```
> 1. 如何实现左侧目录跟右侧文章之间的锚点跳转
```
   在SUMMURY.md文件中
   markdown语法： [目录名](文件路径，相对于自己的book的根目录，受book.json中的root影响)
```
> 2. 如何制作多级目录
```
   在SUMMURY.md中
   文件实现目录结构的设定，在该文件中，可以通过缩进实现多级目录的效果
    * [交接文档](aijianzi/README.md)
        * [管理端](aijianzi/admin/README.md)
          * [课程商品流程](aijianzi/admin/adminCourse.md)
          * [C端直播流程](aijianzi/admin/adminVideo.md )
        * [机构端](aijianzi/agencyAdmin/README.md)
          * [交接](aijianzi/agencyAdmin/agencyAdmin.md)
        * [教师端](aijianzi/agencyTeacher/README.md)
          * [交接](aijianzi/agencyTeacher/agenct.md)
```
> 3. 如何为目录添加序号
```
  gitBook的目录是默认是没有序号的，若想添加编号给目录，book.json中添加配置即可：
  "theme-default": {
     "showLevel": true
  },
```

#### 将gitbook内容托管到 git-pages上
> 1. 建仓库，托管源代码
```
  git init
  git add .
  git commit -m 'readme'
  git remote add origin https://远程仓库地址.git
  git push -u origin master
```
> 2. 使用pages服务展示gitbook书籍
```
  打开git 的pages服务
```