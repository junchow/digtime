# Laravel5.4 Blog 开发

## 一. 框架安装配置

### 1. 安装框架

```
$ cd ~/Code
$ composer create-project laravel/laravel digtime
```
### 2. 配置 homestead

```
$ sudo ~/.homestead/Homestead.yaml
```

配置内容

```
folders:
    - map: ~/Code
       to: /home/vagrant/Code
sites:
    - map: digtime.app
       to: /home/vagrant/Code/digtime/public
database:
    - digtime
variables:
    - key: APP_ENV
      value: local
```

### 3. 重启 Homestead

```
$ vagrant reload --provison
```

### 4. 修改 Hosts 配置

```
$ sudo vim /etc/hosts
```

添加配置

```
192.168.10.10 digtime.app
```

### 5. 通过域名访问

digtime.app

### 6. 进入项目

```
$ vagrant up
$ vagrant ssh
$ cd ~/Code/digtime
```

## 二. 安装项目所需资源

### 1. npm 安装前端包

```
$ npm install
```

npm 安装 n 模块

```
$ npm install -g n
```

升级 NodeJS 到最新版本稳定版

```
sudo n stable
```

### 2. artisan 迁移生成表

```
# 执行所有为执行的迁移
$ php artisan migrate

# 回滚上一次迁移验证
$ php artisan migrate:rollback
```

### 3. artisan 生成权限

```
$ php artisan make:auth
```

### 4. 修改 User 模型位置

```
# 在 app 目录下创建 Models 目录
$ mkdir app/Models

# 将 app 下 User.php 移动到 Models 下
$ mv app/User.php app/Models/User.php
```

修改 `User.php` 的命名空间 namespace 

```
namespace App\Models;
```

全局替换 `App\User` 为 `App\Models\User`

## 三. github 托管项目

```
# 本地新建仓库初始化后生成 .git 目录
$ git init

# 递归地添加当前工作目录中所有文件，在提交之前 git 有一个暂存区(staging area)，可存放新增文件或新增修改，commit 时提交的改动是上一次加入到暂存区中的改动，而非硬盘上的改动。
$ git add -A

# 提交已被 add 进来的改动
$ git commit -m "initial commit"

# 添加一个新的远程仓库，并将项目推送到 github 中
$ git remote add origin git@github.com:junchow/digtime.git

# 将当前分支 merge 到 alias 上的 branch 分支，若分支已存在则更新，若不存在则添加分支。
# 若当前分支与多个主机存在追踪关系，使用 -u 指定默认主机，以后即可不加任何参数使用 git push
$ git push -u origin master

# 迁出一个分支的特定版本，默认迁出分支的 head 版本
$ git checkout master

# 创建并切换分支
$ git checkout -b users
```

## 四. Webpack 打包资源

Laravel5.4 之前的版本用 gulp 的 laravel-elixir 管理全段资源，Laravel5.4 开始使用 webpack 的 Laravel Mix 来管理。

Laravel Mix 提供了一套流式 API，使用通用 CSS 和 JS 预处理器为 Laravel 应用定义 Webpack 构建步骤。

Mix 是位于 Webpack 顶层的配置层，要运行 Mix 任务需在运行包含在默认 package.json 文件中的其中某个 NPM 脚本即可。

```
# 安装 package.json 包`
npm install

# 运行所有 mix 任务
npm run dev

# 运行所有 mix 任务并减少输出
npm run production

# 监控前端资源改变，持续在终端运行并监听所有相关文件的修改，webpack 将会在发现修改后自动重新编译资源文件。
npm run watch
```

## 五. 自定义函数

在 app 目录下创建公共函数 `app/Support/helpers.php`

在 `composer.json` 文件中自动加载
```
"autoload":{
    "files":["app/Support/helpers.php"]
}
```
重新加载方法
```
$ composer dump-autoload
```

## 七. 安装 Markdown 编辑器

使用 npm 安装 markdown 编辑器

```
npm install simplemde --save
```

静态资源中载入
`resources/assets/js/bootstrap.js` 中引入下载的资源包

```
// 引入 markdown 编辑器 simplemde 
window.simplemde = require('simplemde');
```

编译静态资源

```
$ npm run dev
```