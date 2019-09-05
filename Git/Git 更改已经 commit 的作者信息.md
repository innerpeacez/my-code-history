### 使用脚本更改已经 commit 的作者信息

我们创建了一个脚本，该脚本将更改以前在其作者或提交者字段中具有旧电子邮件地址的任何提交，以使用正确的名称和电子邮件地址。

**注意**：运行此脚本会重写所有存储库协作者的历史记录。完成这些步骤后，任何拥有叉子或克隆的人都必须获取重写的历史记录，并将任何本地更改重新绑定到重写的历史记录中。

在运行此脚本之前，您需要：

- 您要更改的作者/提交者字段中显示的旧电子邮件地址
- 您希望将此类提交归因于的正确名称和电子邮件地址

1. 打开Git Bash。

2. 为您的存储库创建一个全新的裸克隆：

   ```shell
   git clone https://github.com/user/repo.git
   cd repo
   ```

3. 复制并粘贴脚本，根据您收集的信息替换以下变量：

   - `OLD_EMAIL` 原来的Email
   - `CORRECT_NAME` 修改的用户名
   - `CORRECT_EMAIL` 修改的Emal

   ```shell
   #!/bin/sh
   
   git filter-branch --env-filter '
   
   OLD_EMAIL="your-old-email@example.com"
   CORRECT_NAME="Your Correct Name"
   CORRECT_EMAIL="your-correct-email@example.com"
   
   if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
   then
       export GIT_COMMITTER_NAME="$CORRECT_NAME"
       export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
   fi
   if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
   then
       export GIT_AUTHOR_NAME="$CORRECT_NAME"
       export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
   fi
   ' --tag-name-filter cat -- --branches --tags
   ```

4. 按Enter键运行脚本。

5. 查看新的Git历史记录以查找错误。

6. 将更正的历史记录推送到GitHub：

   ```shell
   git push --force --tags origin 'refs/heads/*'
   ```

7. 清理临时克隆：

   ```shell
   cd ..
   rm -rf repo.git
   ```

8. 去原来的 clone 仓库拉去修改后的内容

   ```shell
   git pull --allow-unrelated-histories
   ```

   