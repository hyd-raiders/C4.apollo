# 开发改造

```bash
cd xxxx

#添加环境
cat << EOF > src/main/resources/apollo-env.properties
dev.meta=http://devops.dcjet.com.cn:52000/apolloDev
fat.meta=http://devops.dcjet.com.cn:52000/apolloTest
#uat.meta=http://apollo.uat.xxx.com
pro.meta=http://192.168.108.200:52013,http://192.168.108.201:52013
EOF

#删除 application.yml
mv src/main/resources/application.yml src/main/resources/application.yml.bak

#添加 application.properties
cat << EOF > src/main/resources/application.properties
# apollo 中项目ID
app.id=xdo-demo-backend
# 是否开启
apollo.bootstrap.enabled=${apollo:true}
# 支持的namespaces
#apollo.bootstrap.namespaces=application,FX.apollo,application.yml
# meta服务器，请在环境变量配置
#apollo.meta=http://devops.dcjet.com.cn:52000/apolloDev
EOF


# 添加war包支持
mkdir -p src/main/webapp/META-INF/
cat << EOF > src/main/webapp/META-INF/app.properties
#apollo app.id 配置，用于war 启动项目
app.id=xdo-demo-backend
# 支持的namespaces
#apollo.bootstrap.namespaces=application,FX.apollo,application.yml
EOF

```

启动类添加

```java
@EnableApolloConfig
```

build.gradle 添加依赖

```
compile 'com.ctrip.framework.apollo:apollo-client:1.1.0'
```

添加配置

```bash
# windows C:/opt/settings/server.properties
cat << EOF > /opt/settings/server.properties
env=dev
#APOLLO.META=http://devops.dcjet.com.cn:52000/apolloDev
EOF


```



# 纯部署

添加环境配置,根据实际调整 env 值  dev:开发  fat：测试    pro：生产环境

```bash
# windows C:/opt/settings/server.properties
cat << EOF > /opt/settings/server.properties
env=dev
#APOLLO.META=http://devops.dcjet.com.cn:52000/apolloDev
EOF


# 执行jar 或者 war部署 (Dapollo 默认开启)
java -jar -Dapollo=true xxxxxx.jar

# war部署时需要修改log4j2的配置
根据实际情况将log4j2-dev.xml 或者 log4j2-prod.xml 改为 log4j2.xml
```



# 其他特殊配置

#### 本地缓存

- **Mac/Linux**: /opt/data/{*appId*}/config-cache

- **Windows**: C:\opt\data\{*appId*}\config-cache

  注意权限配置

ps ：设置系统变量 APOLLO_CACHEDIR



#### Cluster

一般不建议采用集群

如果使用，设置系统变量 IDC



#### 如何运行

```bash
java -jar -Dapollo=false xdo-demo-api-1.0-SNAPSHOT.jar
```

