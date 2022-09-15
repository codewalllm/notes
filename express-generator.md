---
title: express-generator
tags:
    - 框架
categories:
    - 后端
description:
    - node
---
# 一、全局安装express-generator框架
```
npm i express-generator -g
```
# 二、下载项目框架
### 用的模板是ejs、文件名是blog
```
express --view=ejs blog
```
# 三、安装数据库依赖，sequelize、mysql、和sequelize-cli工具
```
npm i sequelize mysql2 sequelize-cli -S
```
# 四、初始化项目
```
sequelize init
```
# 五、创建数据库
```
sequelize db:create --charset 'utf8mb4'
```
# 六、创建数据库所需要的表
```
sequelize model:generate --name Article --attributes title:string,content:text
```
# 七、运行迁移
```
sequelize db:migrate
```
# 八、新建种子文件
```
sequelize seed:generate --name article
```
# 九、运行迁移
```
sequelize db:seed:all
```