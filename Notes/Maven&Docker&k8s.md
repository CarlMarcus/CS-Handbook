maven/ docker/ k8s 命令

#### maven

mvn -v //查看版本
mvn archetype:create //创建 Maven 项目
mvn compile //编译源代码
mvn test-compile //编译测试代码
mvn test //运行应用程序中的单元测试
mvn site //生成项目相关信息的网站
mvn package //依据项目生成 jar 文件
mvn install //在本地 Repository 中安装 jar
mvn -Dmaven.test.skip=true //忽略测试文档编译
mvn clean //清除目标目录中的生成结果
mvn clean compile //将.java类编译为.class文件
mvn clean package //进行打包
mvn clean test //执行单元测试
mvn clean deploy //部署到版本仓库
mvn clean install //使其他项目使用这个jar,会安装到maven本地仓库中
mvn archetype:generate //创建项目架构
mvn dependency:list //查看已解析依赖
mvn dependency:tree //看到依赖树
mvn dependency:analyze //查看依赖的工具
mvn help:system //从中央仓库下载文件至本地仓库
mvn help:active-profiles //查看当前激活的profiles
mvn help:all-profiles //查看所有profiles
mvn help:effective -pom //查看完整的pom信息
————————————————
版权声明：本文为CSDN博主「耗子他大哥」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Zheng_xiao_xin/article/details/80732865



#### Docker

```
docker logs -f <容器名orID>	查看容器日志
docker ps	查看正在运行的容器
docker ps -a	为查看所有的容器，包括已经停止的
docker rm <容器名orID> 	删除容器

docker stop <容器名orID>	停止
docker start <容器名orID>	启动
docker kill <容器名orID>	杀死

docker images	查看所有镜像
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)	删除所有镜像

docker run --name redmine -p 9003:80 -p 9023:22 -d -v /var/redmine/files:/redmine/files -v /var/redmine/mysql:/var/lib/mysql sameersbn/redmine
运行一个新容器，同时为它命名、端口映射、文件夹映射。以redmine镜像为例

docker pull <镜像名:tag>	拉取镜像

当需要把一台机器上的镜像迁移到另一台机器的时候，需要保存镜像与加载镜像。
机器a
docker save busybox-1 > /home/save.tar
使用scp将save.tar拷到机器b上，然后：
docker load < /home/save.tar

docker build -t <镜像名> <Dockerfile路径>	构建自己的镜像

sudo docker cp 7bb0e258aefe:/etc/debian_version .	从container中拷贝文件出来
```

#### k8s

kubectl get 获取集群的一些信息，如kubectl get pods -o wide显示所有所有pods详细信息

kubectl cluster-info 查看K8S集群信息

kubectl get nodes 获取节点相应服务的信息

kubectl logs xxx 显示pod运行中，容器内程序输出到标准输出的内容 

kubectl rolling-update 不中断业务的更新方式

kubectl run 运行一个pod

kubectl exec 在一个已经运行的容器中执行一条shell命令

删除某个Pod命令节点 `kubectl delete pod <pod名>`

