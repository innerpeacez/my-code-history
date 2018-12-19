阿里Maven仓库

```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>        
</mirror>
```

华为Maven仓库

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