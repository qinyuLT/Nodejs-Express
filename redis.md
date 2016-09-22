//xshell共享文件</br>
http://www.linuxidc.com/Linux/2015-05/117975.htm</br>
  yum install -y lrzsz  rz sz</br>

//linux下redis安装</br>
http://www.cnblogs.com/_popc/p/3684835.html</br>
http://blog.csdn.net/rachel_luo/article/details/8858250</br>

//nodejs 操作 redis</br>
http://www.tuicool.com/articles/UnUrQru</br>


redis数据导入导出
1、安装redis-dump
```c
yum install ruby rubygems ruby-devel      //安装rubygems 以及相关包  
gem sources -a https://ruby.taobao.org/   //源，加入淘宝，外面的源不能访问  
gem install redis-dump -V                 //安装redis-dump
```

导出，密码前面要加一个冒号

redis-dump -u :password@xxx.xxx.xxx.xxx:6379 > redis.json

导入

cat redis.json | redis-load -u :password@localhost
