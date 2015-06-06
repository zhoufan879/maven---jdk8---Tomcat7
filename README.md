Maven 自动化部署
===

.Maven 3.2.5 

.JDK 1.8.0_40 

.Tomcat 7.0.59 

------

1. 配置Tomcat 用户
--------
<code>
	<role rolename="tomcat"/>

  	<role rolename="role1"/>
  	
  	<role rolename="manager-gui"/>
  	
  	<role rolename="manager-script" />
  	
  	<role rolename="admin-gui"/>
  	
  	<user username="tomcat" password="tomcat" roles="tomcat"/>
  	
  	<user username="both" password="tomcat" roles="tomcat,role1"/>
  	
  	<user username="role1" password="tomcat" roles="role1"/>
  	
  	<user username="zhoufan879" password="123456" roles="tomcat,role1,manager-script,admin-gui"/>
  	
</code>

2. 修改Maven的setting.xml
--------
<code>
	<server>
      		<id>tomcat7</id>
      		<username>zhoufan879</username>
      		<password>123456</password>
    	</server>
</code>

3. 配置pom.xml	
--------
<code>
	<build>
		<finalName>StruthioCamelus</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
		                <artifactId>tomcat7-maven-plugin</artifactId>
		                <version>2.2</version>
		                <configuration>
		                    <url>http://localhost:8080/manager/text</url>
		                    <server>tomcat7</server>
		                    <path>/${project.build.finalName}</path>
		                </configuration>
			</plugin>
			
			<plugin>
		                <groupId>org.apache.maven.plugins</groupId>
		                <artifactId>maven-compiler-plugin</artifactId>
		                <version>2.3.2</version>
		                <configuration>
		                    <source>1.8</source>
		                    <target>1.8</target>
		                </configuration>
		        </plugin>
		</plugins>
	</build>
</code>

4. 启动Tomcat 
--------
<code>
./bin/startup.bat
</code>

5. 执行命令
--------
<code>
mvn tomcat7:deploy
</code>

ps: 之后的操作是 redeploy

6. 可能会出现的错误
--------
6.1 编译错误
---------
<code>
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile (default-compile) on project StruthioCamelus: Compilation failure
[ERROR] Unable to locate the Javac Compiler in:
[ERROR] D:\Program Files\Java\jre1.8.0_40\..\lib\tools.jar
[ERROR] Please ensure you are using JDK 1.4 or above and
[ERROR] not a JRE (the com.sun.tools.javac.Main class is required).
[ERROR] In most cases you can change the location of your Java
[ERROR] installation by setting the JAVA_HOME environment variable.
[ERROR] -> [Help 1]
</code>

一般是JDK 配置，选择Eclipse-->window-->preferences-->java-->Installed JREs-->选中JDK，不是JRE
也可能是项目编译环境，工程属性-->project facets-->Dynamic Web Module 3.0 + Java 1.8

6.2 拒绝访问
---------
<code>
[ERROR] Failed to execute goal org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:deploy (default-cli) on project StruthioCamelus: Cannot invoke Tomcat manager: Connection refused: connect -> [Help 1]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
</code>

一般是Tomcat 没启动成功，命令 startup.bat

6.3 Socket出错 
---------
<code>
[ERROR] Failed to execute goal org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:deploy (default-cli) on project StruthioCamelus: Cannot invoke Tomcat manager: Connection reset by peer: socket write error -> [Help 1]
</code>

一般是Tomcat 启动方式不正确，切忌命令 startup.bat，而非tomcat.exe ...神马的


