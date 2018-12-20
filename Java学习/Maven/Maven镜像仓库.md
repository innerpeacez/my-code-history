阿里-Maven镜像仓库

```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>        
</mirror>
```

华为-Maven镜像仓库

```xml
<mirror>
  <id>huaweicloud</id>
  <url>https://mirrors.huaweicloud.com/repository/maven/</url>
  <mirrorOf>*</mirrorOf>  
</mirror>
```
```xml
<server>
  <id>huaweicloud</id>
  <username>anonymous</username>
  <password>devcloud</password>
</server>
```

Jboss-Maven镜像仓库

```xml
<mirror>  
  <id>jboss-public-repository-group</id>
  <mirrorOf>central</mirrorOf>
  <url>http://repository.jboss.org/nexus/content/groups/public  </url>
  <name>JBoss Public Repository Group</name>
</mirror>
```

Repo2-Maven镜像仓库

```xml
<mirror>  
    <id>repo2</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Human Readable Name for this Mirror.</name>  
    <url>http://repo2.maven.org/maven2/</url>  
</mirror>
```

