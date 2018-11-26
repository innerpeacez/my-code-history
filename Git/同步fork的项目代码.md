## 方法一：

使用git命令操作

### 1. Clone your fork:

```
git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git
```

### 2. Add remote from original repository in your forked repository:

```
cd into/cloned/fork-repo
git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/REPO-YOU-FORKED-FROM.git
git fetch upstream
```

### 3. Updating your fork from original repo to keep up with their changes:

```
git pull upstream master
```

### 4.push your update

```
git push
```



## 方法二：

直接在github页面进行操作

![1537431868188](F:\innerpeacez_github\my-code-history\images\%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1537431868188.png)



![1537431928804](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1537431928804.png)



### 修改连接为

```
https://github.com/xxx/xxx/compare/innerpeacez:master...master
```

意思就是比较项目原仓库和你fork的代码仓库的不同

### 创建PR到自己的仓库

![1537432096202](F:\innerpeacez_github\my-code-history\images\%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1537432096202.png)

![1537432074291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1537432074291.png)

### merge PR

![1537432162910](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1537432162910.png)



![1537432190348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1537432190348.png)