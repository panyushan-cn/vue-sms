# lms-vue 

# 后端地址
https://github.com/panyushan-cn/sms

## IDEA 2020.2.1开发

## 运行前准备
第一次运行项目时请先使用<code>npm run build</code>命令执行一次，
并且要导入package.json文件中的所有依赖。否则程序会报缺少依赖错误。
后端采用SpringBoot开发，端口为8998。前端端口为8066。

## 运行时
直接在根目录下使用<code>npm run serve</code>命令运行。页面默认进入根目录。
需要进入`localhost：8066/index`访问。第一次没有登录的本地缓存需要输入密码，
需要根据数据库中存储的信息进行登录。登录后会在本地保存登录状态，下次进入无需登录。

## 结束运行
直接stop程序即可