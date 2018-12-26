引入依赖

```xml
<dependency>
  <groupId>org.eclipse.jgit</groupId>
  <artifactId>org.eclipse.jgit-parent</artifactId>
  <version>5.2.0.201812061821-r</version>
  <type>pom</type>
</dependency>
```

JGit工具类

```java
@Slf4j
public final class GitUtil {
    private String localPath, localGitPath, remotePath;
    private Repository localRepository;
    private String username;
    private String password;
    private Git git;

    public GitUtil(String localPath, String remotePath,String username ,String password) {
        this.username = username;
        this.password = password;
        this.localPath = localPath;
        this.remotePath = remotePath;
        this.localGitPath = this.localPath + "/.git";
        try {
            localRepository = new FileRepository(localGitPath);
        } catch (IOException e) {
            e.printStackTrace();
        }
        git = new Git(localRepository);

    }

    /**
     * 创建本地仓库
     *
     * @throws IOException
     */
    public void create() throws IOException {
        Repository repository = new FileRepository(localGitPath);
        repository.create();
        log.info("create success");
    }

    /**
     * clone克隆远程分支到本地
     *
     * @param branchName
     * @throws GitAPIException
     */
    public void cloneBranch(String branchName) throws GitAPIException {
        Git.cloneRepository()
                .setURI(remotePath)
                .setBranch(branchName)
                .setDirectory(new File(localPath)).call();
        log.info("clone success");
    }

    /**
     * pull远程代码
     *
     * @param branchName 远程分支名称
     * @throws Exception
     */
    public void pull(String branchName) throws Exception {
        git.pull().setRemoteBranchName(branchName).call();
        log.info("pull success");
    }

    /**
     * 将单个文件加入Git
     *
     * @param fileName 添加文件名
     * @throws Exception
     */
    public void add(String fileName) throws Exception {
        File myFile = new File(localPath + fileName);
        myFile.createNewFile();
        git.add().addFilepattern(fileName).call();
        log.info("add success");
    }

    /**
     * 将增加的所有文件加入Git
     *
     * @throws Exception
     */
    public void addAll() throws Exception {
        git.add().addFilepattern(".").call();
        log.info("add success");
    }


    /**
     * 提交文件
     *
     * @param message 提交备注
     * @throws Exception
     */
    public void commit(String message) throws Exception {
        git.commit().setMessage(message).call();
        log.info("commit success");
    }

    /**
     * 同步远程仓库
     *
     * @throws Exception
     */
    public void push() throws Exception {
        git.push().setCredentialsProvider(new UsernamePasswordCredentialsProvider(this.username, this.password)).call();
        log.info("push success");
    }
}
```