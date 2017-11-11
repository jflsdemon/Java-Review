

###打包war
- 右击项目 - Export - war file - 选择保存路径

###部署war
- 把项目打包出的war复制到${tomcat.home}\webapps目录下.当tomcat启动时会自动将其解包. 没有其他设置的情况下，访问的路径就是http://localhost:8080/war name/
- 如果需要修改访问路径，修改${tomcat.home}\conf\server.xml文件，在Host节点下增加如下参考代码:

    **\<Context docBase="D:\pafalearning\userapp\dist\tomcat\userapp.war" path="/userapp" reloadable="true"/>**
    - docBase:指向项目的根目录所在的路径,由于我将项目打成了war包,所以直接指向这个war包就可以了(我的项目名为:userapp).
    - path:是一个虚拟目录,这里设置成了"userapp",则启动Tomcat后,你将通过http://localhost:8080/userapp/*.jsp来访问项目的相关页面.
    - reloadable:如果设置为"true",则表示当你修改jsp文件后,不需要重启服务器就可以实现页面显示的同步. 

### 那为什么war包可以讲项目这个发布出来呢？
1.因为所有新建的文件夹都在WebRoot文件夹下

2.所有的页面都在WebRoot文件夹下

3.所有的后台代码都编译成了 .class文件，在WebRoot \ WEB-INF \ classes 下面

4.我们习惯将所有的 .jar包放在WebRoot \ WEB-INF \ lib 下面

有了这些，就相当于一个项目完全考到了tomcat下面，这就是用war包发布项目的原理
