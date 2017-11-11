### tomcat位置
- /Library／apache-tomcat-9.0.1

### 修改port
- 在{tomcat.home}/conf下的server.xml文件，修改配置文件中的Connector节点的port属性进行的端口修改，修改后重启tomcat后就可以使用新端口访问了。
    - \<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
    
### 启动和关闭
- 在{tomcat.home}/bin下，运行./startuo.sh
- 在{tomcat.home}/bin下，运行./shutdown.sh

### Manage tomcat
- 在{tomcat.home}/conf下，修改tomcat-users.xml，在\<tomcat-users>b标签下添加
    - \<role rolename="manager-gui"/>
    - \<user password="tomcat" roles="manager-gui" username="tomcat"/>

### 
    