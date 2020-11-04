## docker跑通jar错误记录

### docker inspect mysql

#### 1、在docker容器中，两个容器之间的通信通过不同的ip

#### 2、使用命令docker inspect mysql 查看MySQL容器的ip，

#### 3、在springboot项目中的配置文件中修改url连接ip，改为MySQL容器ip

#### 4、jar打包时，跳过测试，直接打包