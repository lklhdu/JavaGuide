# Maven 相关设置

修改位置在`apache-maven-3.3.9\conf\settings.xml`文件中

1. 修改仓库位置

2. 使用阿里云镜像

   ```xml
   <mirrors>
       <mirror>
      <id>alimaven</id>
         <name>aliyun maven</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
         <mirrorOf>central</mirrorOf>        
       </mirror>
     </mirrors>
   ```
   
3. 添加如下配置，解决Plugins导入失败问题

   ```xml
   <profiles>
       <profile>
           <id>aliyun</id>
           <repositories>
               <repository>
                   <id>nexus</id>
                   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                   <releases>
                       <enabled>true</enabled>
                   </releases>
                   <snapshots>
                       <enabled>false</enabled>
                   </snapshots>
               </repository>
           </repositories>
           <pluginRepositories>
               <pluginRepository>
                   <id>admin</id>
                   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                   <releases>
                       <enabled>true</enabled>
                   </releases>
                   <snapshots>
                       <enabled>false</enabled>
                   </snapshots>
               </pluginRepository>
           </pluginRepositories>
       </profile>
   </profiles>
   
   <activeProfiles>
       <activeProfile>aliyun</activeProfile>
   </activeProfiles>
   ```

4. 在IDEA中对maven进行相关设置

## Maven Project

### 解决Idea中的解决junit @RunWith和@Test无法使用的问题

去掉在pom.xml中的<junit>依赖中加入的<scope>test</scope>

### GroupId && ArtifactId

`groupId`一般分为多个段，这里只说两段，第一段为域，第二段为公司名称。域又分为`org`、`com`、`cn`等等许多，其中`org`为非营利组织，`com`为商业组织。

比如创建一个项目，我一般会将`groupId`设置为`cn.cloud.lkl`，`cn`表示域为中国，`lkl`是我个人姓名缩写，`artifactId`设置为`testProj`，表示这个项目的名称是`testProj`，依照这个设置，包结构最好是`cn.snowin.testProj`打头的，如果有个`StudentDao`，它的全路径就是`cn.snowin.testProj.dao.StudentDao`