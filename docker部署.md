# docker简易部署
最重要的一步：想要重新部署项目的时候，必须先停掉自己的项目，要不然端口会被占用，停止的命令如下：
```
[root@server oasystem]# docker ps
CONTAINER ID        IMAGE                              COMMAND                  CREATED             STATUS              PORTS                                                                                            NAMES
cf1374538f21        springboot-role/oasystem           "java -Djava.secur..."   18 minutes ago      Up 18 minutes       0.0.0.0:8081->8181/tcp                                                                           agitated_easley
2952f5546808        springboot-user-company/oasystem   "java -Djava.secur..."   35 minutes ago      Up 35 minutes       0.0.0.0:8082->8181/tcp                                                                           sad_agnesi
b4338e1e9a1a        springboot-menu/oasystem           "java -Djava.secur..."   38 minutes ago      Up 38 minutes       0.0.0.0:8083->8181/tcp                                                                           dazzling_jepsen
a559b64f050c        lynckia/licode                     "./licode/extras/d..."   18 hours ago        Up 18 hours         0.0.0.0:3000-3001->3000-3001/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:30000-30050->30000-30050/udp   licode
[root@server oasystem]#
```
找到自己的CONTAINER ID，使用下面的命令
```
[root@server oasystem]# docker stop cf1374538f21
```
即可停掉容器

第一步：进入自己的文件夹，
```
[root@server projectoa]# pwd
/projectoa
[root@server projectoa]# ls
menu  role  usr-company
```
如果修改不是很多，比如一个或者几个类的话，建议替换，如果是多个文件的话，建议整个文件夹一起替换掉

第二步：替换掉文件之后，进入下面的目录
```
[root@server projectoa]# cd menu
[root@server menu]# cd oasystem/
[root@server oasystem]# pwd
/projectoa/menu/oasystem
```
输入命令
```
[root@server oasystem]# mvn package docker:build
```
开始构建，看到build success之后完成

第三步：使用下面的命令，查看自己刚刚构建的东西
```
[root@server oasystem]# docker images
REPOSITORY                                                  TAG                            IMAGE ID            CREATED             SIZE
springboot-role/oasystem                                    latest                         3851b47de14c        6 minutes ago       696 MB
springboot-user-company/oasystem                            latest                         6951804e1521        23 minutes ago      696 MB
springboot-menu/oasystem                                    latest                         3a8024e8c189        27 minutes ago      177 MB
```

第四步：
1.使用下面的命令启动项目，注意，role是8081，user-company是8082，menu是8083，，，后面的8181不需要改
2.修改后面的springboot-role/oasystem为自己的镜像，也就是第三步中自己构建的东西
```
[root@server oasystem]# docker run -p 8081:8181 -t springboot-role/oasystem
```

后续：随便玩，反正机器炸不了，有问题随时@我
