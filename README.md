# 使用docker部署springboot项目
第一步：在pom文件中添加docker.image.prefix
```
<properties>
        <java.version>1.8</java.version>
        <docker.image.prefix>springboot-menu</docker.image.prefix>
</properties>
```
第二步：添加maven插件
```
<!-- Docker maven plugin -->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.0.0</version>
                <configuration>
                    <imageName>${docker.image.prefix}/oasystem</imageName>
                    <dockerDirectory>src/main/docker</dockerDirectory>
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>
                </configuration>
            </plugin>
<!-- Docker maven plugin -->
```
第三步：在main文件夹下面添加docker文件夹，里面Dockerfile，为什么是在main下面？因为第二步规定了src/main/docker，注意这个文件没有后缀名，可以使用txt进行编辑，然后删除后缀

第四步：把整个项目放到服务器上，注意并不是jar包，之前的话，应该都是直接把jar包放到服务器上，现在不是，现在是整个文件夹

第五步：进入和pom文件平级的文件夹里面，使用mvn package docker:build命令构建镜像，什么是镜像，不知道

第六步：使用docker run -p 8080:8080 -t springboot/spring-boot-docker命令运行镜像，也就是启动一个容器，springboot/spring-boot-docker为镜像名称，也可以是id，两个端口需要解释一下，第一个8080端口是服务器的端口，第二个端口是容器的端口，容器的端口即项目的端口，也就是boot项目设置的端口

效果：同时启动多个项目，端口都是8080，由于容器隔离，没问题，不会报错，但是需要服务器的不同端口指向这几个项目

